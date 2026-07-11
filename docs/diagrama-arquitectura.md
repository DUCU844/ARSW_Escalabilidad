# Diagrama de arquitectura

Arquitectura objetivo del laboratorio: tráfico de Internet entrando por un Application
Load Balancer, distribuido hacia un Target Group con health checks en `/health`, cuyas
instancias son administradas por un Auto Scaling Group repartido en dos zonas de
disponibilidad.

```mermaid
flowchart TD
    U["Usuario / navegador / curl"] --> ALB["Application Load Balancer\nalb-scalability-ha\nInternet-facing"]
    ALB --> TG["Target Group\ntg-scalability-ha\nHealth check: /health"]
    TG --> A["EC2 Web A\nAZ 1"]
    TG --> B["EC2 Web B\nAZ 2"]
    TG -.escala bajo carga.-> C["EC2 Web C\nAZ 1 o AZ 2"]

    ASG["Auto Scaling Group\nasg-web-scalability\nmin 2 / desired 2 / max 3\nTarget tracking CPU 50%"] -. administra .-> A
    ASG -. administra .-> B
    ASG -. administra .-> C

    subgraph SG_ALB["sg-alb-scalability"]
        ALB
    end
    subgraph SG_EC2["sg-ec2-scalability"]
        A
        B
        C
    end
```

## Componentes y responsabilidad

| Componente | Responsabilidad |
|---|---|
| Application Load Balancer | Recibe tráfico HTTP público y lo distribuye entre targets saludables |
| Target Group | Agrupa instancias y ejecuta health checks (`/health`) para decidir quién recibe tráfico |
| Auto Scaling Group | Lanza/termina instancias EC2 según capacidad deseada, health checks y política de escalamiento |
| Launch Template | Plantilla (AMI, tipo de instancia, Security Group) usada por el ASG para lanzar instancias |
| CloudWatch | Recolecta métricas (CPU, RequestCount, HealthyHostCount, etc.) que alimentan la política de escalamiento y la observabilidad |

Este diagrama corresponde a la sección 4 de la guía oficial del laboratorio.

# Reto Final: Escalabilidad, Alta Disponibilidad y Observabilidad en AWS

**Asignatura:** Arquitecturas de Software
**Estudiante:** _(completa tu nombre)_
**Plataforma:** AWS Academy Learner Lab
**Región:** _(ej. us-east-1)_

---

## 1. Diagrama de arquitectura implementada

```
Usuario / Navegador / curl
          |
          v
Application Load Balancer (alb-scalability-ha)
DNS público
          |
          v
Target Group (tg-scalability-ha)
Health check: /health
          |
     +----+----+----+
     v    v    v
   EC2   EC2   EC2
  Web A Web B Web C
 AZ-1  AZ-2  (si escala)

Auto Scaling Group (asg-web-scalability)
Min: 2 | Max: 3 | Desired: 2
Política: Target Tracking - CPU promedio 50%
```

Componentes creados:

| Elemento | Nombre | Función |
|---|---|---|
| Security Group ALB | `sg-alb-scalability` | HTTP (80) desde `0.0.0.0/0` |
| Security Group EC2 | `sg-ec2-scalability` | HTTP (80) únicamente desde `sg-alb-scalability` |
| AMI | `ami-web-scalability-arsw` | Imagen base con Apache y stress-ng |
| Launch Template | `lt-web-scalability` | Plantilla de lanzamiento para el ASG |
| Target Group | `tg-scalability-ha` | Registro automático de instancias, health check en `/health` |
| Load Balancer | `alb-scalability-ha` | Internet-facing, distribuye tráfico HTTP |
| Auto Scaling Group | `asg-web-scalability` | Min 2, Max 3, Desired 2, política de CPU al 50% |

---

## 2. Captura del Auto Scaling Group

_(pega aquí tu captura: `EC2 → Auto Scaling Groups → asg-web-scalability`, mostrando instancias e Instance management)_

---

## 3. Captura del Load Balancer

_(pega aquí tu captura: `EC2 → Load Balancers → alb-scalability-ha`, estado `Active`)_

---

## 4. Captura del Target Group con targets Healthy

_(pega aquí tu captura: `EC2 → Target Groups → tg-scalability-ha → Targets`, ambos targets en `Healthy`)_

---

## 5. Evidencia de respuesta desde varias instancias

_(pega aquí varias respuestas de `curl http://DNS_DEL_ALB` o capturas del navegador, mostrando distinto `Instance ID` y `Availability Zone` en cada una)_

---

## 6. Evidencia de escalamiento

_(pega aquí cómo generaste la carga — `stress-ng --cpu 2 --timeout 300s` por SSH, o solicitudes HTTP en bucle — y la captura/salida de `Activity` del Auto Scaling Group mostrando el evento de lanzamiento y el cambio de `DesiredCapacity`)_

---

## 7. Evidencia de métricas en CloudWatch

_(pega aquí al menos 3 métricas con sus valores antes / durante / después de la carga: `CPUUtilization`, `HealthyHostCount`, `RequestCount`, `TargetResponseTime` o `GroupDesiredCapacity`, con sus gráficas o tabla de valores)_

---

## 8. Evidencia de falla simulada y recuperación

_(pega aquí la instancia que detuviste/terminaste, el estado `unhealthy` en el Target Group, y el evento del Auto Scaling Group lanzando el reemplazo. Incluye si el ALB siguió respondiendo durante la prueba)_

---

## 9. Análisis de escalabilidad

_(responde: qué política de escalamiento usaste y sobre qué métrica; qué disparó el evento de escalamiento; qué limitación tiene usar solo CPU como métrica; qué pasó cuando la carga terminó — ¿hubo scale-in?)_

---

## 10. Análisis de alta disponibilidad

_(responde: qué mecanismos sostienen la disponibilidad en esta arquitectura — múltiples AZ, health checks, reemplazo automático; qué diferencia hay entre ocultar una falla y recuperarse de ella; qué observaste durante tu prueba de falla)_

---

## 11. Análisis de observabilidad

_(responde: qué métricas revisaste y cómo se correlacionan con los eventos de carga/falla; qué limitaciones tiene la observabilidad disponible en este entorno académico, sin CloudWatch Agent ni IAM personalizado)_

---

## 12. Propuesta de mejora para producción

- HTTPS con AWS Certificate Manager (ACM): listener 443 con certificado gestionado, redirigiendo HTTP a HTTPS.
- Instancias en subnets privadas, con el ALB como único punto de entrada y un NAT Gateway para tráfico saliente.
- Métrica de escalamiento más representativa: combinar CPU con `RequestCountPerTarget`, más cercana a la experiencia real del usuario.
- CloudWatch Alarms explícitas que notifiquen a un canal de operaciones ante escalamientos o caídas de instancias.
- Logs centralizados: access logs del ALB hacia S3, y CloudWatch Agent para métricas de memoria y logs de aplicación.
- Despliegues sin downtime vía Instance Refresh, con `MinHealthyPercentage` controlado.
- Base de datos altamente disponible (RDS Multi-AZ o Aurora) si la aplicación tuviera persistencia.
- Infraestructura como código (Terraform/CloudFormation) para reproducibilidad y control de versiones.

---

## Rúbrica de referencia

| Criterio | Descripción | Peso |
|---|---|---|
| Escalabilidad | Configura un Auto Scaling Group con política de escalamiento. | 25% |
| Alta disponibilidad | Usa ALB, Target Group y al menos dos zonas de disponibilidad. | 20% |
| Observabilidad | Analiza métricas relevantes en CloudWatch. | 20% |
| Prueba de falla | Simula caída de instancia y analiza la recuperación. | 15% |
| Análisis arquitectónico | Relaciona decisiones con atributos de calidad. | 15% |
| Limpieza de recursos | Elimina recursos para evitar consumo innecesario. | 5% |

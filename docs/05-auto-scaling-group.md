# Parte 2 — Auto Scaling Group (secciones 15-18 de la guía)

## Qué hace este componente

El Auto Scaling Group (ASG) es el que **decide cuántas instancias deben existir** y las
mantiene: lanza instancias desde el Launch Template, las registra automáticamente en el
Target Group, y si el ALB reporta una como no saludable, la reemplaza. Es el componente
central tanto de escalabilidad (política de escalamiento) como de alta disponibilidad
(reemplazo automático de instancias fallidas).

| Parámetro | Valor |
|---|---|
| Launch template | `lt-web-scalability`, versión `Latest` |
| Subnets | Las dos subnets públicas (2 AZ) |
| Target group | `tg-scalability-ha` (attach, no registro manual) |
| Health check type | Elastic Load Balancing health checks: **Enabled** |
| Health check grace period | 120s |
| Min / Desired / Max | 2 / 2 / 3 |
| Política de escalamiento | Target tracking, Average CPU utilization, target 50 |

Habilitar los health checks de ELB importa: si se dejara solo el status check de EC2,
el ASG únicamente miraría si la instancia está `running`, no si `/health` responde bien
detrás del ALB.

## Pasos en consola

`EC2 → Auto Scaling Groups → Create Auto Scaling group`:

1. Name: `asg-web-scalability`. Launch template: `lt-web-scalability`, Latest.
2. Red: VPC por defecto, selecciona las dos subnets públicas.
3. Load balancing: `Attach to an existing load balancer` → `Choose from your load
   balancer target groups` → `tg-scalability-ha`.
4. Health checks: `Turn on Elastic Load Balancing health checks` → Enabled. Health
   check grace period: 120 segundos.
5. Capacidad: Desired capacity 2, Minimum capacity 2, Maximum capacity 3.
6. Scaling policy: `Target tracking scaling policy`, Metric type: `Average CPU
   utilization`, Target value: 50.

## Verificación

- `EC2 → Instances`: deben aparecer 2 instancias nuevas creadas por el ASG (nombre de
  tag `web-scalability-asg` o similar), distintas a la instancia base.
- `EC2 → Target Groups → tg-scalability-ha → Targets`: espera hasta que ambas
  instancias aparezcan como `Healthy` (puede tardar 1-2 minutos por el grace period y
  los health checks iniciales).
- Copia el DNS del ALB: `EC2 → Load Balancers → alb-scalability-ha → DNS name`, y prueba
  en el navegador o con:

```bash
for i in {1..10}; do
  curl -s http://DNS_DEL_LOAD_BALANCER | grep "Instance ID"
done
```

El `Instance ID` debe alternar entre las dos instancias del ASG.

## Checklist

- [ ] ASG `asg-web-scalability` con 2 instancias `InService` / `Healthy`.
- [ ] Ambas instancias aparecen `Healthy` en `tg-scalability-ha`.
- [ ] `http://DNS_DEL_LOAD_BALANCER` responde y alterna entre Instance IDs distintos.
- [ ] Política de target tracking (CPU 50%) asociada al ASG.

Registra el DNS del ALB, capturas del ASG/targets y tus respuestas de `curl` en
[`evidencias/05-auto-scaling-group.md`](../evidencias/05-auto-scaling-group.md).

## Actividad 1 — análisis de escalabilidad y alta disponibilidad

Responde en [`docs/actividades/actividad1-escalabilidad-ha.md`](actividades/actividad1-escalabilidad-ha.md).

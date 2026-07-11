# Parte 4 — Observabilidad con CloudWatch (secciones 23-26 de la guía)

## Métricas del Load Balancer

`CloudWatch → Metrics → All metrics → ApplicationELB`. Filtra por tu load balancer
(`alb-scalability-ha`) y revisa:

- `RequestCount`
- `HealthyHostCount` / `UnHealthyHostCount`
- `TargetResponseTime`
- `HTTPCode_Target_2XX_Count`
- `HTTPCode_Target_5XX_Count`

## Métricas del Auto Scaling Group

`EC2 → Auto Scaling Groups → asg-web-scalability → Monitoring`:

- `Group desired capacity`
- `Group in service instances`
- `Group total instances`
- `Average CPU utilization`

## Métricas de EC2

`CloudWatch → Metrics → EC2`, filtra por las instancias del ASG:

- `CPUUtilization`
- `NetworkIn` / `NetworkOut`
- `StatusCheckFailed`

## Cómo comparar antes / durante / después de la carga

Antes de generar la carga (Parte 3), anota los valores actuales de al menos 3
métricas. Genera la carga, y vuelve a mirar las mismas métricas mientras dura y unos
minutos después de que termine. En CloudWatch puedes ajustar el rango de tiempo del
gráfico (arriba a la derecha) a "Last 1 hour" y el período a 1 o 5 minutos para ver el
cambio con claridad.

## Checklist

- [ ] `RequestCount`, `HealthyHostCount`, `TargetResponseTime` revisadas en `ApplicationELB`.
- [ ] `Group desired capacity` revisada en el Auto Scaling Group.
- [ ] `CPUUtilization` revisada en EC2, antes/durante/después de la carga (Parte 3).

Guarda capturas de las gráficas en [`evidencias/07-observabilidad.md`](../evidencias/07-observabilidad.md).

## Actividad 3 — análisis de observabilidad

Responde en [`docs/actividades/actividad3-observabilidad.md`](actividades/actividad3-observabilidad.md),
usando al menos tres métricas de las anteriores.

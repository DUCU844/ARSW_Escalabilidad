# Informe técnico — Laboratorio de Escalabilidad, Alta Disponibilidad y Observabilidad

Corresponde al reto final (sección 32 de la guía). Reemplaza cada sección con tu
contenido antes de entregar. Reutiliza las capturas y notas que ya guardaste en
`evidencias/` a medida que avanzabas por cada parte.

## 1. Diagrama de arquitectura implementada

_(Puedes reutilizar/adaptar [`docs/diagrama-arquitectura.md`](diagrama-arquitectura.md) con tus IDs reales.)_

## 2. Captura del Auto Scaling Group

## 3. Captura del Load Balancer

## 4. Captura del Target Group con targets Healthy

## 5. Evidencia de respuesta desde varias instancias

_(Salida de `curl` repetido mostrando distintos `Instance ID`; ver `evidencias/05-auto-scaling-group.md`.)_

## 6. Evidencia de escalamiento o intento de escalamiento

_(Captura de la pestaña `Activity` del ASG; ver `evidencias/06-pruebas-escalabilidad.md`.)_

## 7. Evidencia de métricas en CloudWatch

_(Ver actividad 3 en [`docs/actividades/actividad3-observabilidad.md`](actividades/actividad3-observabilidad.md).)_

## 8. Evidencia de falla simulada y recuperación

_(Ver actividad 4 en [`docs/actividades/actividad4-alta-disponibilidad.md`](actividades/actividad4-alta-disponibilidad.md).)_

## 9. Análisis de escalabilidad

## 10. Análisis de alta disponibilidad

## 11. Análisis de observabilidad

## 12. Propuesta de mejora para producción

_(Ver ideas de partida en [`docs/10-limpieza.md`](10-limpieza.md#cierre-del-laboratorio), sección "Cierre".)_

---

## Rúbrica de referencia (sección 33)

| Criterio | Descripción | Peso |
|---|---|---|
| Escalabilidad | Configura un Auto Scaling Group con política de escalamiento. | 25% |
| Alta disponibilidad | Usa ALB, Target Group y al menos dos zonas de disponibilidad. | 20% |
| Observabilidad | Analiza métricas relevantes en CloudWatch. | 20% |
| Prueba de falla | Simula caída de instancia y analiza la recuperación. | 15% |
| Análisis arquitectónico | Relaciona decisiones con atributos de calidad. | 15% |
| Limpieza de recursos | Elimina recursos para evitar consumo innecesario. | 5% |

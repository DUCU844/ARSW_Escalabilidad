# Laboratorio: Escalabilidad, Alta Disponibilidad y Observabilidad en AWS

Asignatura: Arquitecturas de Software (ARSW). Guía paso a paso para completar el
laboratorio de Auto Scaling + Application Load Balancer + CloudWatch **en la consola
de AWS Academy Learner Lab**, sin roles IAM personalizados.

Este repo es una bitácora del laboratorio, no una automatización: la idea es que
hagas cada paso tú mismo en la consola (para realmente aprender el flujo) y vayas
dejando evidencia de lo que hiciste.

## Cómo usarlo

1. Sigue los documentos de `docs/` **en orden** (`00` a `10`). Cada uno trae la teoría
   necesaria y los pasos exactos de consola para esa parte de la guía oficial.
2. A medida que completas cada parte, registra tu evidencia (IDs reales, DNS, capturas)
   en el archivo correspondiente de [`evidencias/`](evidencias/README.md).
3. Responde las actividades de análisis en [`docs/actividades/`](docs/actividades) según
   avances (no al final).
4. Al terminar, arma el informe técnico a partir de
   [`docs/informe-tecnico-template.md`](docs/informe-tecnico-template.md).

## Progreso del laboratorio

- [ ] Parte 0 – Preparación (`docs/00-preparacion.md`)
- [ ] Parte 1 – Security Groups (`docs/01-security-groups.md`)
- [ ] Parte 1 – Instancia base y User Data (`docs/02-instancia-base.md`)
- [ ] Parte 1 – AMI y Launch Template (`docs/03-ami-launch-template.md`)
- [ ] Parte 2 – Target Group y Load Balancer (`docs/04-target-group-alb.md`)
- [ ] Parte 2 – Auto Scaling Group (`docs/05-auto-scaling-group.md`)
- [ ] Parte 3 – Prueba de escalabilidad (`docs/06-pruebas-escalabilidad.md`)
- [ ] Parte 4 – Observabilidad con CloudWatch (`docs/07-observabilidad.md`)
- [ ] Parte 5 – Alta disponibilidad (`docs/08-alta-disponibilidad.md`)
- [ ] Diagnóstico de errores e informe técnico (`docs/09-diagnostico-errores.md`, `docs/informe-tecnico-template.md`)
- [ ] Limpieza de recursos (`docs/10-limpieza.md`)

## Estructura

```
docs/              Guías paso a paso por consola, por parte del laboratorio
docs/actividades/  Plantillas de las actividades 1-4 y tabla de relación de conceptos
evidencias/        Un archivo por parte para registrar IDs, DNS, tiempos y capturas reales
```

## Advertencia de costos

El Application Load Balancer consume créditos mientras esté activo, tenga o no
tráfico. No lo dejes encendido al terminar: sigue [`docs/10-limpieza.md`](docs/10-limpieza.md).

# Parte 5 — Prueba de alta disponibilidad (secciones 27-29 de la guía)

## Qué se va a comprobar

Que **detener una instancia no tumba el servicio**: el ALB deja de enviarle tráfico en
cuanto el health check falla, y el ASG la reemplaza para volver a la capacidad deseada.
Esa es la diferencia entre *ocultar* una falla (el usuario no la nota, porque otra
instancia responde) y *recuperarse* de ella (el sistema vuelve a su capacidad original).

## Pasos en consola

1. `EC2 → Instances`, selecciona una de las instancias creadas por el Auto Scaling
   Group (no la instancia base).
2. `Instance state → Stop instance`.
3. Espera unos minutos y observa:
   - `EC2 → Target Groups → tg-scalability-ha → Targets`: el target detenido debe
     pasar a `unhealthy` y luego desaparecer de la lista.
   - `EC2 → Auto Scaling Groups → asg-web-scalability → Activity`: debe aparecer un
     evento de terminar la instancia no saludable y lanzar un reemplazo.
   - Navegador o `curl http://DNS_DEL_LOAD_BALANCER`: el servicio debe seguir
     respondiendo durante todo el proceso.

Esto puede tardar varios minutos: primero el health check tiene que fallar (2 fallos ×
15s de intervalo), luego el ASG detecta la instancia no saludable y lanza el
reemplazo, y esa nueva instancia debe pasar sus propios health checks antes de recibir
tráfico.

## Checklist

- [ ] Instancia detenida manualmente.
- [ ] El Target Group la marcó `unhealthy` y luego la retiró.
- [ ] `http://DNS_DEL_LOAD_BALANCER` siguió respondiendo durante la prueba.
- [ ] El ASG lanzó una instancia de reemplazo para volver a `desired capacity`.

Registra capturas del target caído/reemplazado y de la pestaña `Activity` en
[`evidencias/08-alta-disponibilidad.md`](../evidencias/08-alta-disponibilidad.md).

## Actividad 4 — análisis de alta disponibilidad

Responde en [`docs/actividades/actividad4-alta-disponibilidad.md`](actividades/actividad4-alta-disponibilidad.md).

## Relación entre los tres conceptos (sección 30)

Completa la tabla en [`docs/actividades/tabla-relacion-conceptos.md`](actividades/tabla-relacion-conceptos.md).

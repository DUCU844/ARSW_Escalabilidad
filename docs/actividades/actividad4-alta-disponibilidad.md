# Actividad 4 — Análisis de alta disponibilidad

Corresponde a la sección 29 de la guía.

1. **¿Qué ocurrió cuando se detuvo una instancia?**
   _Respuesta:_

2. **¿El Load Balancer siguió respondiendo?**
   _Respuesta:_

3. **¿El Target Group detectó la falla?**
   _Respuesta:_

4. **¿El Auto Scaling Group lanzó una nueva instancia?**
   _Respuesta:_

5. **¿Qué diferencia existe entre ocultar una falla y recuperarse de una falla?**
   _Respuesta:_

6. **¿Qué atributo de calidad se evidencia en esta prueba?**
   _Respuesta:_

> Pista: "ocultar" es que el usuario no note nada porque el tráfico ya iba a otra
> instancia sana (eso lo da el ALB en el momento); "recuperarse" es que el sistema
> restaura su capacidad original reemplazando lo que falló (eso lo da el ASG, no de
> forma instantánea). El atributo de calidad que se evidencia es disponibilidad
> (availability), sostenido por redundancia + detección de fallos + auto-remediación.

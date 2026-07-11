# Actividad 1 — Análisis de escalabilidad y alta disponibilidad

Corresponde a la sección 19 de la guía. Completa cada respuesta con lo que observaste
en tu propio despliegue (nombres de recursos, IDs, comportamiento real).

1. **¿Qué componente distribuye el tráfico?**
   _Respuesta:_

2. **¿Qué componente decide cuántas instancias deben existir?**
   _Respuesta:_

3. **¿Qué componente verifica la salud de las instancias?**
   _Respuesta:_

4. **¿Por qué se seleccionan dos zonas de disponibilidad?**
   _Respuesta:_

5. **¿Qué diferencia existe entre Target Group y Auto Scaling Group?**
   _Respuesta:_

6. **¿Qué pasaría si una instancia falla?**
   _Respuesta:_

7. **¿Qué pasaría si aumenta la carga?**
   _Respuesta:_

> Pista general: el Target Group solo sabe "quién está sano ahora mismo" (ejecuta el
> health check y expone el resultado al ALB); el ASG sabe "cuántas instancias debería
> haber" y actúa (lanza/termina) según capacidad deseada, health checks y políticas de
> escalamiento. Son responsabilidades distintas aunque están conectados.

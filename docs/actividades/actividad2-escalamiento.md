# Actividad 2 — Análisis del escalamiento

Corresponde a la sección 22 de la guía. Completa con tus valores reales observados en
`describe-scaling-activities` / la pestaña `Activity` del ASG.

1. **¿Qué métrica activó la política de escalamiento?**
   _Respuesta:_

2. **¿Cuántas instancias había antes de la prueba?**
   _Respuesta:_

3. **¿Cuántas instancias hubo después?**
   _Respuesta:_

4. **¿Cuánto tiempo tardó el sistema en reaccionar?**
   _Respuesta:_

5. **¿Qué limitación tiene usar solo CPU como métrica de escalamiento?**
   _Respuesta:_

6. **¿Qué otra métrica podría ser útil para una aplicación web?**
   _Respuesta:_

> Pista: una app puede estar saturada (colas largas, latencia alta) sin que la CPU se
> vea afectada, por ejemplo si el cuello de botella es I/O, red o una dependencia
> externa lenta. `RequestCountPerTarget` o `TargetResponseTime` reflejan mejor la
> experiencia real del usuario que `CPUUtilization`.

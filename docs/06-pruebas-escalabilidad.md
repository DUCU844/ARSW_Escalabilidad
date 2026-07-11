# Parte 3 — Prueba de escalabilidad (secciones 20-22 de la guía)

## Generar carga

**Alternativa A — CPU real vía SSH (más confiable para disparar el escalamiento).**
Requiere que la instancia tenga el puerto 22 abierto y un key pair asociado. Conéctate
a una de las instancias del ASG y ejecuta:

```bash
ssh -i tu-key.pem ec2-user@IP_PUBLICA_DE_UNA_INSTANCIA_DEL_ASG
stress-ng --cpu 2 --timeout 300s
```

Esto satura CPU durante 5 minutos, tiempo suficiente para que CloudWatch recolecte
métricas y la política de target tracking reaccione.

**Alternativa B — solicitudes HTTP desde tu equipo (no requiere SSH):**

```bash
for i in {1..500}; do
  curl -s http://DNS_DEL_LOAD_BALANCER/load.html > /dev/null &
done
wait
```

Como Apache sirve un HTML estático muy rápido, esta alternativa **puede no elevar la
CPU lo suficiente** para cruzar el umbral del 50% configurado en la política. Es un
resultado válido y esperado: forma parte del análisis de la Actividad 2 (ver
"limitación de usar solo CPU").

## Observar el escalamiento

`EC2 → Auto Scaling Groups → asg-web-scalability`, revisa las pestañas:

- **Activity**: busca eventos de tipo "Launch" que sigan a la carga.
- **Instance management**: cuenta cuántas instancias hay en este momento.
- **Monitoring**: gráfica de `Average CPU utilization` y `Group desired capacity`.

Busca que `Desired capacity` pase de `2` a `3`. El escalamiento puede tardar varios
minutos: CloudWatch agrega métricas en ventanas de tiempo y la política de target
tracking no reacciona instantáneamente a un pico momentáneo.

## Checklist

- [ ] Carga generada (SSH+stress-ng o HTTP en bucle).
- [ ] Pestaña `Activity` del ASG revisada; se registró (o no) un evento de scale-out.
- [ ] `Desired capacity` observado antes y después de la carga.

Registra tiempos, capturas de `Activity`/`Monitoring` y el resultado en
[`evidencias/06-pruebas-escalabilidad.md`](../evidencias/06-pruebas-escalabilidad.md).

## Actividad 2 — análisis del escalamiento

Responde en [`docs/actividades/actividad2-escalamiento.md`](actividades/actividad2-escalamiento.md).

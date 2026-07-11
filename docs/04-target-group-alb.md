# Parte 2 — Target Group y Application Load Balancer (secciones 13-14 de la guía)

## Target Group primero, sin registrar targets a mano

El Target Group (`tg-scalability-ha`) define **cómo** se decide si una instancia está
sana (health check HTTP en `/health`, cada 15s, 2 aciertos para pasar a `Healthy`, 2
fallos para pasar a `Unhealthy`). No registres instancias manualmente: eso lo hará
el Auto Scaling Group en el siguiente paso, automáticamente, cada vez que lance o
termine una instancia.

| Parámetro | Valor |
|---|---|
| Target type | Instances |
| Protocol / Port | HTTP / 80 |
| Health check path | `/health` |
| Healthy threshold | 2 |
| Unhealthy threshold | 2 |
| Timeout | 5s |
| Interval | 15s |
| Success codes | 200 |

## Application Load Balancer

- Internet-facing, IPv4.
- **Dos zonas de disponibilidad** (con sus subnets públicas): si una AZ completa tiene
  un problema, el ALB sigue enrutando por la otra.
- Security Group: `sg-alb-scalability`.
- Listener `HTTP:80` → forward a `tg-scalability-ha`.

## Pasos en consola

1. `EC2 → Target Groups → Create target group` con los parámetros de la tabla.
   VPC: la VPC por defecto. Protocol version: HTTP1.
2. `EC2 → Load Balancers → Create load balancer → Application Load Balancer`:
   - Name: `alb-scalability-ha`. Scheme: Internet-facing. IP address type: IPv4.
   - Network mapping: VPC por defecto, selecciona las **dos** AZ/subnets públicas.
   - Security group: `sg-alb-scalability`.
   - Listener `HTTP:80` → Forward to `tg-scalability-ha`.
3. Crea el Load Balancer y espera a que su estado pase a `Active`.

## Verificación

`EC2 → Target Groups → tg-scalability-ha → Targets`: en este punto es normal que no
haya targets todavía; se poblarán cuando exista el Auto Scaling Group
([`05-auto-scaling-group.md`](05-auto-scaling-group.md)).

## Checklist

- [ ] Target Group `tg-scalability-ha` creado con health check en `/health`.
- [ ] ALB `alb-scalability-ha` en estado `active`, en 2 AZ, con `sg-alb-scalability`.
- [ ] Listener `HTTP:80` reenviando a `tg-scalability-ha`.

Registra ARNs/DNS y capturas en [`evidencias/04-target-group-alb.md`](../evidencias/04-target-group-alb.md).

# Parte 1 — Security Groups (sección 8 de la guía)

Se crean dos Security Groups. La idea de arquitectura: **el EC2 solo confía en el ALB**,
no en Internet directamente. Por eso `sg-ec2-scalability` no abre el puerto 80 a
`0.0.0.0/0`, sino solo al Security Group del balanceador.

## sg-alb-scalability

| Regla | Tipo | Protocolo | Puerto | Origen |
|---|---|---|---|---|
| Entrada | HTTP | TCP | 80 | `0.0.0.0/0` |
| Salida | All traffic | All | All | `0.0.0.0/0` (regla por defecto) |

## sg-ec2-scalability

| Regla | Tipo | Protocolo | Puerto | Origen |
|---|---|---|---|---|
| Entrada | HTTP | TCP | 80 | `sg-alb-scalability` (por ID de grupo, no CIDR) |
| Entrada (opcional) | SSH | TCP | 22 | Tu IP (`My IP`), solo si necesitas depurar |

No se necesita SSH para completar el laboratorio: el bootstrap de Apache se hace con
User Data (ver [`02-instancia-base.md`](02-instancia-base.md)).

## Pasos en consola

1. `EC2 → Security Groups → Create security group`.
2. Crea `sg-alb-scalability`:
   - VPC: la VPC por defecto.
   - Regla de entrada: HTTP, puerto 80, origen `0.0.0.0/0`.
3. Crea `sg-ec2-scalability`:
   - VPC: la misma VPC por defecto.
   - Regla de entrada: HTTP, puerto 80, origen = **el Security Group `sg-alb-scalability`**
     (en el buscador de origen, escribe el nombre o el `sg-xxxx` del grupo, no un CIDR).

## Verificación

`EC2 → Security Groups → sg-ec2-scalability → Inbound rules`: la regla de entrada
HTTP/80 debe mostrar como "Source" el Security Group `sg-alb-scalability` (no
`0.0.0.0/0`).

## Checklist

- [ ] `sg-alb-scalability` creado, entrada HTTP/80 desde Internet.
- [ ] `sg-ec2-scalability` creado, entrada HTTP/80 **solo** desde `sg-alb-scalability`.

Registra tus IDs reales (`sg-...`) en [`evidencias/01-security-groups.md`](../evidencias/01-security-groups.md).

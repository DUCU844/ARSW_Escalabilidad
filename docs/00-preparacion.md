# Parte 0 — Preparación

## Conceptos que se ponen en práctica en este laboratorio

- **Escalabilidad horizontal**: agregar más instancias EC2 para repartir carga, en vez
  de agrandar una sola máquina (escalabilidad vertical).
- **Alta disponibilidad**: que el servicio siga respondiendo aunque una instancia falle,
  gracias al Load Balancer y a que hay más de una instancia en más de una zona de
  disponibilidad (AZ).
- **Observabilidad**: poder explicar *por qué* el sistema se comporta como se comporta,
  usando métricas de CloudWatch (CPU, conteo de requests, targets saludables, etc.).

Ver diagrama completo en [`diagrama-arquitectura.md`](diagrama-arquitectura.md).

## Antes de empezar

1. Entra a **AWS Academy Learner Lab** y abre la consola de AWS ("AWS Details" → `AWS console`).
2. Escoge **una sola región** y no la cambies durante todo el laboratorio.
   Esta guía usa `us-east-1` como ejemplo.
3. Verifica que exista una **VPC por defecto** con subnets públicas en al menos dos
   zonas de disponibilidad: consola `VPC → Your VPCs` (debe haber una marcada
   `Default: Yes`), y `VPC → Subnets` filtrando por esa VPC para confirmar que hay
   subnets en al menos 2 AZ con auto-assign public IP habilitado.
4. Restricciones de AWS Academy que aplican todo el laboratorio:
   - No se crean IAM roles ni instance profiles personalizados (`IAM instance profile: None`).
   - No se usa SSM Session Manager, CloudFormation, Terraform, RDS ni Route 53.

## Cómo usar este repo

Sigue los documentos de `docs/` en orden (`00` a `10`); cada uno tiene los pasos
exactos de consola para esa parte de la guía oficial. A medida que completes cada
parte, registra tu evidencia (capturas + IDs reales de tus recursos) en el archivo
correspondiente de `evidencias/` y marca la casilla en el checklist del
[`README.md`](../README.md).

## Checklist de esta parte

- [ ] Sesión de Learner Lab activa y consola abierta.
- [ ] Región elegida y fijada (no se cambiará en todo el laboratorio).
- [ ] VPC por defecto confirmada, con subnets públicas en ≥ 2 AZ.

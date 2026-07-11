# Limpieza de recursos (sección 34) y cierre (sección 35)

## Por qué importa

El Application Load Balancer consume créditos de AWS Academy mientras esté activo,
**aunque no reciba tráfico**. Por eso no debe quedar encendido después del laboratorio.

## Orden de eliminación (consola)

1. `EC2 → Auto Scaling Groups → asg-web-scalability → Delete`.
2. `EC2 → Load Balancers → alb-scalability-ha → Actions → Delete`.
3. `EC2 → Target Groups → tg-scalability-ha → Actions → Delete`.
4. `EC2 → Launch Templates → lt-web-scalability → Actions → Delete template`.
5. `EC2 → AMIs → ami-web-scalability-arsw → Actions → Deregister AMI`.
6. `EC2 → Snapshots`: elimina el snapshot asociado a esa AMI.
7. `EC2 → Instances → web-scalability-base → Instance state → Terminate` (si sigue existiendo).
8. `EC2 → Security Groups`: elimina `sg-ec2-scalability` primero (referencia a
   `sg-alb-scalability` como origen de su regla), luego `sg-alb-scalability`.

## Verificación final

- `EC2 → Auto Scaling Groups`: no debe aparecer `asg-web-scalability`.
- `EC2 → Load Balancers`: no debe aparecer `alb-scalability-ha`.
- `EC2 → Instances`: ninguna instancia del laboratorio en estado distinto a `terminated`.
- `EC2 → Security Groups`: `sg-alb-scalability` y `sg-ec2-scalability` ya no existen.

## Checklist

- [ ] Ningún Auto Scaling Group activo del laboratorio.
- [ ] Ningún Load Balancer activo del laboratorio.
- [ ] Ningún Target Group, Launch Template, AMI ni snapshot del laboratorio.
- [ ] Ninguna instancia EC2 del laboratorio en estado distinto a `terminated`.
- [ ] Security Groups `sg-alb-scalability` y `sg-ec2-scalability` eliminados.

Registra la evidencia de limpieza (capturas de las listas vacías) en
[`evidencias/10-limpieza.md`](../evidencias/10-limpieza.md).

## Cierre del laboratorio

Este laboratorio integró tres capacidades fundamentales de una arquitectura cloud:

- **Escalabilidad**: aumentar o reducir capacidad según la demanda (Auto Scaling Group
  + política de target tracking).
- **Alta disponibilidad**: mantener el servicio operativo ante fallas (ALB + Target
  Group + múltiples AZ + reemplazo automático de instancias).
- **Observabilidad**: entender el comportamiento del sistema mediante métricas
  (CloudWatch).

La arquitectura implementada no solo distribuye tráfico entre instancias: también
permite que el sistema crezca ante carga, detecte instancias no saludables y entregue
información para diagnosticar su comportamiento.

### Posibles mejoras para producción

- HTTPS con ACM (certificado + listener 443).
- Instancias privadas detrás del ALB (subnets privadas + NAT Gateway o VPC Endpoints).
- Base de datos Multi-AZ (RDS).
- CloudWatch Alarms sobre las métricas ya observadas.
- Logs centralizados (ALB access logs, CloudWatch Logs).
- Despliegues blue/green.
- Auto Scaling basado en `RequestCountPerTarget` en vez de (o además de) CPU.
- Infraestructura como código (Terraform/CloudFormation), fuera del alcance de este
  laboratorio por las restricciones de AWS Academy.

El aprendizaje principal: la escalabilidad no se diseña de forma aislada. Una
arquitectura escalable también debe ser observable y disponible.

# Diagnóstico de errores comunes (sección 31 de la guía)

## El Target Group muestra instancias `Unhealthy`

- ¿La ruta `/health` existe y responde `200`? Pruébalo desde tu navegador con la IP
  pública de la instancia: `http://IP_INSTANCIA/health`.
- ¿Apache está activo en la instancia?
- ¿`sg-ec2-scalability` permite tráfico en el puerto 80 **desde `sg-alb-scalability`**
  (por Security Group, no por CIDR)?
- ¿El Target Group usa protocolo HTTP, puerto 80?
- ¿Las instancias están en la misma VPC que el Target Group?

## El Auto Scaling Group no crea instancias

- ¿El Launch Template referencia una AMI que sigue disponible (`Available`)?
- ¿El tipo de instancia (`t2.micro`/`t3.micro`) está permitido en AWS Academy?
- ¿Hay subnets seleccionadas en la configuración de red del ASG?
- ¿Se alcanzó el límite de instancias de la cuenta/región?
- ¿La capacidad deseada es mayor que 0?

Revisa `EC2 → Auto Scaling Groups → asg-web-scalability → Activity` para ver el
mensaje de error exacto de cualquier intento fallido de lanzamiento.

## No se activa el escalamiento

- ¿La carga duró suficiente tiempo (varios minutos, no segundos)?
- ¿La métrica CPU superó realmente el umbral del 50%? (ver `docs/07-observabilidad.md`).
- ¿La política de target tracking sigue asociada al ASG? (`Automatic scaling` en la
  consola del ASG).
- ¿El máximo del grupo (`Maximum capacity`) permite crecer más allá de 2?
- ¿CloudWatch ya recolectó suficientes puntos de datos? (la política de target
  tracking necesita varias muestras, no reacciona a un solo pico).

## El Load Balancer no responde

- ¿El ALB es `Internet-facing`?
- ¿`sg-alb-scalability` permite HTTP/80 desde `0.0.0.0/0`?
- ¿El Target Group tiene al menos un target `Healthy`?
- ¿Copiaste bien el DNS name del ALB (`EC2 → Load Balancers → alb-scalability-ha →
  DNS name`), no el ARN ni el nombre?

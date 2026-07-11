# Parte 1 — Instancia base y User Data (secciones 9-10 de la guía)

Esta instancia **no** es una de las que atenderán tráfico en producción del laboratorio:
es una plantilla desechable que se usa una sola vez para generar la AMI
([`03-ami-launch-template.md`](03-ami-launch-template.md)). Una vez creada la AMI,
esta instancia se puede detener o terminar.

## Qué hace el User Data

Al arrancar, el script:

1. Actualiza paquetes e instala `httpd` (Apache) y `stress-ng` (para la prueba de carga
   de la Parte 3).
2. Habilita y arranca Apache.
3. Consulta el metadata service (IMDSv2, con token) para obtener `instance-id` y
   `availability-zone` de la propia instancia.
4. Genera tres archivos en `/var/www/html/`:
   - `index.html`: página con el Instance ID y la AZ, para comprobar visualmente
     que el Load Balancer está repartiendo tráfico entre instancias distintas.
   - `health`: responde `OK`. Es el endpoint que usará el health check del Target Group.
   - `load.html`: endpoint simple para la prueba de carga con `curl`.

## Pasos en consola

1. `EC2 → Instances → Launch instances`.
2. `Name`: `web-scalability-base`.
3. AMI: **Amazon Linux 2023**. Tipo: `t2.micro`/`t3.micro`.
4. VPC/subnet: la VPC por defecto, una subnet pública. Auto-assign public IP: **Enable**.
5. Security Group: `sg-ec2-scalability`. IAM instance profile: **None**.
6. En `Advanced details → User data`, pega:

```bash
#!/bin/bash
dnf update -y
dnf install -y httpd stress-ng
systemctl enable httpd
systemctl start httpd

TOKEN=$(curl -X PUT "http://169.254.169.254/latest/api/token" \
  -H "X-aws-ec2-metadata-token-ttl-seconds: 21600")
INSTANCE_ID=$(curl -H "X-aws-ec2-metadata-token: $TOKEN" \
  http://169.254.169.254/latest/meta-data/instance-id)
AZ=$(curl -H "X-aws-ec2-metadata-token: $TOKEN" \
  http://169.254.169.254/latest/meta-data/placement/availability-zone)

cat <<EOF > /var/www/html/index.html
<!DOCTYPE html>
<html>
<head>
    <title>Laboratorio de Escalabilidad</title>
    <style>
        body { font-family: Arial, sans-serif; background: #f4f7fb; color: #1f2937; padding: 40px; }
        .card { background: white; border-radius: 12px; padding: 30px; max-width: 760px; margin: auto; box-shadow: 0 10px 25px rgba(0,0,0,0.08); }
        h1 { color: #2563eb; }
        code { background: #f3f4f6; padding: 4px 8px; border-radius: 6px; }
        .badge { display: inline-block; background: #dbeafe; color: #1d4ed8; padding: 6px 12px; border-radius: 20px; font-weight: bold; }
    </style>
</head>
<body>
    <div class="card">
        <span class="badge">ARSW - Escalabilidad</span>
        <h1>Aplicacion Web Escalable</h1>
        <p>Esta respuesta fue generada por una instancia EC2 dentro de un Auto Scaling Group.</p>
        <p><strong>Instance ID:</strong> <code>$INSTANCE_ID</code></p>
        <p><strong>Availability Zone:</strong> <code>$AZ</code></p>
        <p><strong>Estado:</strong> Servicio disponible</p>
    </div>
</body>
</html>
EOF

echo "OK" > /var/www/html/health

cat <<EOF > /var/www/html/load.html
<!DOCTYPE html>
<html>
<head><title>Simulacion de carga</title></head>
<body>
    <h1>Endpoint de prueba de carga</h1>
    <p>Use este endpoint para generar solicitudes HTTP.</p>
</body>
</html>
EOF
```

7. Lanza la instancia y espera a que quede en estado `Running` con status checks `2/2`.

## Verificación

Copia la IP pública de la instancia (`EC2 → Instances → web-scalability-base`) y prueba
desde tu navegador o con `curl`:

```
http://IP_PUBLICA
http://IP_PUBLICA/health
```

`/health` debe responder `OK`. Si no responde, revisa
[`09-diagnostico-errores.md`](09-diagnostico-errores.md).

## Checklist

- [ ] Instancia `web-scalability-base` en estado `running`, status checks `2/2`.
- [ ] `http://IP_PUBLICA` responde la página con Instance ID y AZ.
- [ ] `http://IP_PUBLICA/health` responde `OK`.

Registra IP pública, Instance ID y capturas en [`evidencias/02-instancia-base.md`](../evidencias/02-instancia-base.md).

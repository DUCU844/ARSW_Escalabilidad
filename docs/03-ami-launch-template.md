# Parte 1 — AMI y Launch Template (secciones 11-12 de la guía)

## Por qué una AMI

El Auto Scaling Group necesita poder lanzar instancias **idénticas** bajo demanda, en
segundos, sin depender de que un User Data termine de instalar paquetes cada vez (eso
sería lento y frágil frente a fallas de red o de repositorios). Por eso se "hornea" la
instancia base validada en una AMI: Apache y los archivos (`index.html`, `health`,
`load.html`) quedan ya incluidos en el disco de arranque.

**Importante:** el Launch Template que usa esta AMI **no** lleva User Data. Si lo
llevara, el script volvería a ejecutarse en cada instancia nueva (no hace daño, pero es
redundante e innecesario).

## Pasos en consola

1. Con la instancia base validada (`/health` responde `OK`):
   `EC2 → Instances → web-scalability-base → Actions → Image and templates → Create image`.
2. `Image name`: `ami-web-scalability-arsw`. Descripción: "Imagen base para laboratorio
   de escalabilidad ARSW". Crea la imagen y espera a que quede en estado `Available`
   (`EC2 → AMIs`).
3. `EC2 → Launch Templates → Create launch template`:
   - Name: `lt-web-scalability`.
   - Template version description: "Web server for scalability lab".
   - AMI: selecciona `ami-web-scalability-arsw`.
   - Instance type: `t2.micro`/`t3.micro`.
   - Security Group: `sg-ec2-scalability`.
   - IAM instance profile: **None**.
   - **No agregues User Data.**

## Verificación

`EC2 → AMIs`: `ami-web-scalability-arsw` en estado `Available`.
`EC2 → Launch Templates`: `lt-web-scalability` existe y su versión más reciente apunta
a esa AMI.

## Checklist

- [ ] AMI `ami-web-scalability-arsw` en estado `Available`.
- [ ] Launch Template `lt-web-scalability` creado, sin User Data, con `sg-ec2-scalability`.
- [ ] (Opcional) instancia base detenida/terminada para ahorrar créditos, ya que no se
      volverá a usar.

Registra IDs de la AMI y el Launch Template en [`evidencias/03-ami-launch-template.md`](../evidencias/03-ami-launch-template.md).

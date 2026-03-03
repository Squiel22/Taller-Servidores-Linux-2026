Taller de Servidores Linux – Febrero 2026

Automatización de despliegue de:

Servidor NFS (CentOS Stream 9)

Cliente NFS con autofs (Ubuntu Server)

Servicio HTTP sobre share NFS

Hardening básico y firewall

Topología de VM's y red:

Red utilizada:
192.168.10.0/24

VM's

nfs01	CentOS Stream 9	192.168.10.100	Servidor NFS

app01	Ubuntu Server	192.168.10.101	Cliente NFS + Webserver


Requisitos Previos

En el nodo de control (Ubuntu):

sudo apt update

sudo apt install ansible -y

Verificar instalación:

ansible --version

Instalar colecciones necesarias:

ansible-galaxy collection install -r collections/requirements.yml

Configuración de acceso SSH:

Ansible utiliza SSH para conectarse a los servidores remotos.

ssh-keygen

ssh-copy-id obligatorio@192.168.10.100

Verificar acceso:

ssh obligatorio@192.168.10.100


Cómo ejecutar:

ansible-playbook -i inventory/hosts.ini site.yml --ask-become-pass (es necesario agregar --ask-become-pass al final ya que Ansible ejecutará en sudo y nos solicitará la contraseña, esto debido a que en los Playbooks tenemos become: true)

Verificación en nfs server (nfs01):

1 . Verificar que el servicio NFS esté activo

systemctl is-active nfs-server

2 . Verificar exportaciones configuradas

exportfs -v

3 . Verificar firewall

firewall-cmd --list-services | egrep 'nfs|mountd|rpc-bind'

4️ . Verificar archivo compartido

ls -l /srv/nfs/shared/README-NFS.txt

Verificación en el cliente (app01)

1 . Verificar servicio autofs

systemctl is-active autofs

2️ . Antes de acceder al recurso (no debería estar montado)

mount | grep /mnt/shared

3 . Forzar el automontaje

ls /mnt/shared

4 . Verificar que el recurso esté montado

mount | grep /mnt/shared

Verificación en el webserver (app01)

1️ .  Verificar servicio http activo

systemctl is-active shared-http

2️ . Probar acceso web

curl http://localhost:8080/README-NFS.txt

Evidencias de funcionamiento:

<img width="1153" height="205" alt="image" src="https://github.com/user-attachments/assets/2bc3a96d-aed3-446f-b297-17c986517bd9" />


<img width="968" height="110" alt="image" src="https://github.com/user-attachments/assets/3a5944d9-94eb-42ee-b0e3-eb7cd9f61488" />


<img width="866" height="946" alt="image" src="https://github.com/user-attachments/assets/4d6a3e4a-4239-4bab-85a2-53bfbcea3793" />



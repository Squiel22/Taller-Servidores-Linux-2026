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




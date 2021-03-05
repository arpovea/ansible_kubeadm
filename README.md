# Despliegue de Cluster de Kubernetes con ansible utilizando Kubeadm

En este repositorio se encuentra, un escenario de Vagrant que contiene la configuración para levantar tres máquina con `kvm/libvirt`, ademas de un playbook de `ansible` para desplegar un cluster de Kubernetes con kubeadm que tiene un nodo "Controller" y dos nodos "Workers".

## Puesta en marcha:

Levantaremos el escenario de las tres máquinas con Vagrant, para ello una vez estes en la ruta de este repositorio realiza lo siguiente:

`vagrant up`

Luego en un entorno virtual de `python3` instalaremos los paquetes necesarios que se encuentran en el fichero `requirements.txt`, una vez activado el entorno virtual realiza:

`pip install -r requirements.txt`

Cuando esto termine tendremos instalado `ansible` en nuestro entorno virtual, ahora para realizar el despligue y configuración del cluster de Kubernetes realiza lo siguiente:

`ansible-playbook -b site.yml` 

## Configuracíon ansible.

En este apartado repasaremos los puntos mas importantes o pequeños detalles en la configuración de este "playbook" de ansible.

### Roles


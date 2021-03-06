# Despliegue de Cluster de Kubernetes con ansible utilizando Kubeadm

En este repositorio se encuentra, un escenario de Vagrant que contiene la configuración para levantar tres máquina con `kvm/libvirt`, ademas de un playbook de `ansible` para desplegar un cluster de Kubernetes con kubeadm que tiene un nodo "Controller" y dos nodos "Workers".

## Puesta en marcha:

Levantaremos el escenario de las tres máquinas con Vagrant, para ello una vez estes en la ruta de este repositorio realiza lo siguiente:

`vagrant up`

Luego en un entorno virtual de `python3` instalaremos los paquetes necesarios que se encuentran en el fichero `requirements.txt`, una vez activado el entorno virtual realiza:

`pip install -r requirements.txt`

Cuando esto termine tendremos instalado `ansible` en nuestro entorno virtual, ahora para realizar el despligue y configuración del cluster de Kubernetes realiza lo siguiente:

`ansible-playbook -b site.yml` 

## Configuracíon ansible:

En este apartado repasaremos los puntos mas importantes o pequeños detalles en la configuración de este "playbook" de ansible.

### Roles

**Rol "commons":**

Este rol se ejecuta en todas las máquinas, aseguramos que todas las máquinas esten actualizadas, tengan la swap desactivada (Kubernetes no funciona con swap) y que tengan los paquetes necesarios para el despligue en todas ellas, los mas importantes:

* Docker: Es el componente que ejecuta los contenedores.

* Kubeadm: Instalará y configurará los distintos componentes de un clúster de forma estándar.

* Kubelet: Servicio que se ejecuta en todos los nodos y gestiona operaciones a nivel de nodo.

**Rol "controlador":**

Este rol se ejecuta en la máquina que será nuestro nodo controlador, instalamos el paquete `kubectl`, iniciamos el cluster (indicandole la subred que se va utilizar para los distintos "pods" en este caso coincide con la de Flannel) después se crea el directorio ".kube" y se copia el fichero "config" dentro de este para la configuración de `kubectl`. 
Una vez realizado esto se va a desplegar Flannel, que sera el encargado de la subred de los "pods", Por último se genera un "token" para que los "workers" se puedan agregar al cluster, este se mete en una variable de ansible para utilizarla posteriormente en los "workers" (para hacer uso de esta variable es necesario que en el fichero `ansible.cfg` agregemos el siguiente valor: `gather_facts=false`)

* Kubectl: se utiliza para emitir comandos al clúster a través de su servidor API.

* Flannel: es una red superpuesta muy simple que satisface los requisitos de Kubernetes. Mucha gente ha informado del éxito con Flannel y Kubernetes

**Rol "workers":**

Este rol es muy simple, ya que únicamente ejecutamos la variable que se ha creado en el rol "controlador" para añadir los workers al cluster

#### Notas finales

En algunas plays de los distintos roles podéis observar que se realizan distintas redirecciones a ficheros, esto simplemente es para que no se pierda dicha información ya que nos puede ser útil para distintas tareas.

En esta tarea se ha utilizado la autenticación para kubectl de admin/root para no alargar mas la práctica, pero si quieres saber como generar configuraciones para que otros usuarios puedan acceder a tu cluster desde otras máquinas:

https://kubernetes.io/docs/tasks/access-application-cluster/configure-access-multiple-clusters/

### Ejemplo de despligue de una aplicación

Para este pequeño ejemplo se va a utilizar:

https://github.com/iesgn/kubernetes-storm/tree/master/unidad3/ejemplos-3.2/ejemplo8

Desplegamos la aplicación (estando en el directorio ejemplo8):

`kubectl apply -f .`

Para ver que todo funciona correctamente realizamos:

`kubectl get all`

Para ver el ingress:

`kubectl get ingress`
`kubectl describe ingress`

Para escalar letschat:

`kubectl scale deployment letschat --replicas=4`

Probar letschat:

`curl 192.168.50.2:[PUERTOASIGNADO]`

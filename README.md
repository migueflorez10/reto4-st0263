# reto4-st0263
Despliegue de la misma aplicación del Reto 3 en un clúster de alta disponibilidad en Kubernetes en AWS o GCP. Utilizando servicios gestionados como EKS o GKE. Asegura balanceo de carga, alta disponibilidad en la aplicación, base de datos y almacenamiento. Implementando HTTPS con un nombre de dominio.


### Info de la Materia: Topicos Especiales en Telematica-st0263

### Estudiantes:
- Miguel Angel Martinez Florez, mamartinef@eafit.edu.co
- Mateo Muñoz Cadavid, mmunozc4@eafit.edu.co

### Profesor:  Edwin Nelson Montoya Munera, emontoya@eafit.edu.co  

# Reto 4: AKubernetes

##  Objetivo
El objetivo de este reto es desplegar la misma aplicación del Reto 3 en un cluster de alta disponibilidad en Kubernetes, utilizando un servicio 
administrado en la nube como Kubernetes de GCP. Se busca implementar un balanceador de cargas, alta disponibilidad en la capa de aplicación, alta 
disponibilidad en la capa de base de datos y alta disponibilidad en la capa de almacenamiento. Además, se debe implementar un dominio para el servicio 
con certificado SSL gestionado por el grupo.

##  Descripción de la Actividad
- En este reto, debemos utilizar la nube de GCP, donde se deberá instalar un clúster de Kubernetes con un servicio 
administrado en la nube. La aplicación a desplegar es un CMS o LMS dockerizado, similar al desplegado en el Reto 3, pero con mejoras en la disponibilidad y escalabilidad.

- Se debe implementar un balanceador de cargas para distribuir el tráfico entre los nodos del clúster, asegurando así la alta disponibilidad en la capa de
aplicación. Para la base de datos, se puede elegir entre desplegarla dentro del clúster Kubernetes o utilizar un servicio administrado de base de datos en 
la nube, asegurando también la alta disponibilidad.

- En cuanto al sistema de archivos, se puede optar por desplegar el servidor NFS en el propio clúster o utilizar un servicio administrado de NFS en la
nube, garantizando la alta disponibilidad en la capa de almacenamiento.

- Se debe implementar un dominio para el servicio, con el formato https://reto4.dominio.tld, y gestionar el certificado SSL.


##  Arquitectura del Reto
![image](https://github.com/migueflorez10/reto4-st0263/assets/68928440/a7e9f824-08cd-4311-bb71-1f5f39f20965)

La arquitectura del proyecto para desplegar la aplicación del Reto 3 en un clúster de alta disponibilidad en Kubernetes en Google Cloud Platform (GCP) consta de 
varios componentes interconectados para garantizar la escalabilidad y la disponibilidad. A continuación, se detalla cada uno de estos componentes:

- **Aplicación Dockerizada:** La aplicación CMS o LMS está contenida en un contenedor Docker. Este contenedor contiene el código de la aplicación y todas sus
dependencias, lo que facilita su despliegue y mantenimiento en el clúster de Kubernetes.

- **Clúster de Kubernetes en GCP:** Se utiliza un clúster de Kubernetes administrado en GCP para desplegar y gestionar los contenedores de la aplicación. El clúster
está compuesto por varios nodos, cada uno ejecutando un agente de Kubernetes (kubelet) para administrar los contenedores.

- **Balanceador de Cargas:** Se implementa un balanceador de cargas en GCP para distribuir el tráfico de red entre los nodos del clúster de Kubernetes. Esto garantiza
que la aplicación sea accesible y que se pueda escalar horizontalmente para manejar cargas variables.

- **Alta Disponibilidad en la Capa de Aplicación:** Para lograr alta disponibilidad en la capa de aplicación, se despliegan múltiples réplicas de la aplicación en el clúster
de Kubernetes. Kubernetes se encarga de distribuir estas réplicas entre los nodos y de mantener el número deseado de réplicas activas en todo momento.

- **Alta Disponibilidad en la Capa de Base de Datos:** Se puede optar por desplegar una base de datos dentro del clúster de Kubernetes o utilizar un servicio administrado de
base de datos en la nube, como Cloud SQL en GCP. En ambos casos, se configura la base de datos para que sea altamente disponible, utilizando réplicas y configuraciones de
respaldo adecuadas.

- **Alta Disponibilidad en la Capa de Almacenamiento:** Para la capa de almacenamiento, se puede desplegar un servidor NFS dentro del clúster de Kubernetes o utilizar un servicio
administrado de NFS en la nube, como Cloud Filestore en GCP. Esto garantiza que los datos de la aplicación estén disponibles y sean persistentes incluso en caso de fallo
de un nodo o disco.

- **Dominio y Certificado SSL:** Se implementa un dominio para el servicio, con el formato https://reto4.dominio.tld. Se gestiona un certificado SSL para asegurar la comunicación 
segura entre los usuarios y la aplicación.


## Guía paso a paso de instalación y configuración
### Guía de Implementación en Google Cloud Platform (GCP)
Esta guía detalla el proceso de implementación y configuración de los elementos necesarios para un proyecto en GCP. A continuación, se describen los pasos realizados:

### Creación del Clúster de Kubernetes
Para crear el clúster de Kubernetes, siga el tutorial en [este enlace](https://cloud.google.com/kubernetes-engine/docs/tutorials/persistent-disk).

1) Activa Cloud Shell para ejecutar los comandos necesarios.
2) Habilita las APIs de Google Kubernetes Engine (GKE) y Cloud SQL Admin con los siguientes comandos:

```
gcloud services enable container.googleapis.com sqladmin.googleapis.com
```

3) Configura la región predeterminada para la CLI de Google Cloud:
```
gcloud config set compute/region us-central1-a
```

4) Establece la variable de entorno PROJECT_ID con el ID de tu proyecto de Google Cloud
```
export PROJECT_ID=reto-4-422404
```

5) Descarga los archivos de manifiesto de la aplicación desde el repositorio de GitHub:
```
git clone https://github.com/EsteTruji/kubernetes-manifests
```

6) Cambia al directorio con los archivos de manifiesto:
```
cd kubernetes-manifests
```

7) Establece la variable de entorno `WORKING_DIR`: 
```
WORKING_DIR=$(pwd)
```

### Creación del Clúster de GKE

1) Crea un clúster de GKE llamado `persistent-disk-tutorial` (puedes elegir otro nombre):
```
CLUSTER_NAME=persistent-disk-tutorial
gcloud container clusters create $CLUSTER_NAME
```

2) Conéctate a tu nuevo clúster:
```
gcloud container clusters get-credentials $CLUSTER_NAME --region us-central1-a
```

### Configuración de NFS
Para configurar NFS, sigue los pasos del tutorial aquí. Asegúrate de realizar las siguientes tareas:
1) Habilita las APIs de Cloud Filestore y Google Kubernetes Engine.
2) Si deseas usar Google Cloud CLI, instala y luego inicializa `gcloud CLI`. Si ya lo tienes instalado, ejecuta `gcloud components update` para obtener la versión más reciente.
3) Habilita el controlador de CSI de Filestore en un clúster existente con el siguiente comando:
```
gcloud container clusters update CLUSTER_NAME --update-addons=GcpFilestoreCsiDriver=ENABLED
```
4) Crea un PersistentVolume y una PersistentVolumeClaim para acceder a la instancia. Aquí tienes un ejemplo de archivo de manifiesto `(nfs-pv-pvc.yaml)`:
```
apiVersion: v1
kind: PersistentVolume
metadata:
  name: nfs-pv
spec:
  storageClassName: ""
  capacity:
    storage: 1Ti
  accessModes:
    - ReadWriteMany
  persistentVolumeReclaimPolicy: Retain
  volumeMode: Filesystem
  csi:
    driver: filestore.csi.storage.gke.io
    volumeHandle: "modeInstance/us-central1-a  /nfs-server/nfs_share1"
    volumeAttributes:
      ip: 10.34.43.66
      volume: nfs_share1
---
kind: PersistentVolumeClaim
apiVersion: v1
metadata:
  name: podpvc
spec:
  accessModes:
    - ReadWriteMany
  storageClassName: ""
  volumeName: nfs-pv
  resources:
    requests:
      storage: 1Ti

```
5) Ejecuta el siguiente comando para crear los recursos de PersistentVolume y PersistentVolumeClaim:
```
kubectl apply -f nfs-pv-pvc.yaml
```

### Configuración de la Base de Datos

1. En Cloud Shell, crea una instancia denominada `mysql-wordpress-instance`:

```
INSTANCE_NAME=mysql-wordpress-instance
gcloud sql instances create $INSTANCE_NAME
 ```

2) Agrega el nombre de la conexión de la instancia como una variable de entorno:
```
export INSTANCE_CONNECTION_NAME=$(gcloud sql instances describe $INSTANCE_NAME \
    --format='value(connectionName)')
```

3) Crea una base de datos llamada `wordpress` para almacenar los datos de WordPress:
```
gcloud sql databases create wordpress --instance $INSTANCE_NAME
```

4) Crea un usuario de base de datos llamado `wordpress` y establece una contraseña para la autenticación:
```
CLOUD_SQL_PASSWORD=$(openssl rand -base64 18)
gcloud sql users create wordpress --host=% --instance $INSTANCE_NAME \
    --password $CLOUD_SQL_PASSWORD
```

### Configuración de WordPress
- Configuración Inicial: 
Antes de implementar WordPress, sigue estos pasos:
1) Crea una cuenta de servicio para permitir que la aplicación de WordPress acceda a la instancia de MySQL a través de un proxy de Cloud SQL:
```
SA_NAME=cloudsql-proxy
gcloud iam service-accounts create $SA_NAME --display-name $SA_NAME
```

2) Agrega la dirección de correo electrónico de la cuenta de servicio como variable de entorno:
```
SA_EMAIL=$(gcloud iam service-accounts list \
    --filter=displayName:$SA_NAME \
    --format='value(email)')
```

3) Asigna el rol `cloudsql.client` a la cuenta de servicio:
```
gcloud projects add-iam-policy-binding $PROJECT_ID \
    --role roles/cloudsql.client \
    --member serviceAccount:$SA_EMAIL
```

4) Crea una clave para la cuenta de servicio:
```
gcloud iam service-accounts keys create $WORKING_DIR/key.json \
    --iam-account $SA_EMAIL
```

5) Crea un secreto de Kubernetes para las credenciales de MySQL:
```
kubectl create secret generic cloudsql-db-credentials \
    --from-literal username=wordpress \
    --from-literal password=$CLOUD_SQL_PASSWORD
```

6) Crea otro secreto de Kubernetes para las credenciales de la cuenta de servicio:
```
kubectl create secret generic cloudsql-instance-credentials \
    --from-file $WORKING_DIR/key.json
```

### Implementación de WordPress en GKE
1) **Archivo de Manifiesto** `wordpress_cloudsql.yaml`:
- El archivo `wordpress_cloudsql.yaml` describe una implementación que crea un único Pod ejecutando un contenedor con una instancia de WordPress.
- Este contenedor lee la variable de entorno `WORDPRESS_DB_PASSWORD`, que contiene las credenciales del secreto cloudsql-db-credentials que creaste previamente.
- Además, configura el contenedor de WordPress para comunicarse con MySQL a través del proxy de Cloud SQL que se ejecuta en el contenedor sidecar. El valor de la dirección del host se establece en la variable de entorno `WORDPRESS_DB_HOST`.

2) Preparación del Archivo:
- Prepara el archivo reemplazando la variable de entorno `INSTANCE_CONNECTION_NAME`:
```
cat $WORKING_DIR/wordpress_cloudsql.yaml.template | envsubst > \
    $WORKING_DIR/wordpress_cloudsql.yaml
```

3) Implementación del Archivo de Manifiesto:
- Implementa el archivo `wordpress_cloudsql.yaml`:
```
kubectl create -f $WORKING_DIR/wordpress_cloudsql.yaml
```

4) Verificación del Estado:
- Observa la implementación para verificar que el estado cambie a “Running”:
```
kubectl get pod -l app=wordpress --watch
```

5) Exposición del Servicio::
- Crea un servicio de tipo `LoadBalancer`:
```
kubectl create -f $WORKING_DIR/wordpress-service.yaml
```

6) Obtención de la Dirección IP Externa:
- Observa el servicio y espera a que se le asigne una dirección IP externa:
```
kubectl get svc -l app=wordpress --watch
```

7) Configuración del Blog/Página:
- En tu navegador, ve a la siguiente URL, reemplazando `external-ip-address` con la dirección IP externa del servicio que expone tu instancia de WordPress:
```
http://external-ip-address
```

8) Pasos de Configuración:
- En la página de instalación de WordPress, selecciona un idioma y haz clic en “Continuar”.
- Completa la página de información necesaria y haz clic en “Instalar WordPress”.
- Haz clic en “Iniciar sesión”.
- Ingresa el nombre de usuario y contraseña que creaste anteriormente.

9) Visualización de Recursos en el Cluster de Kubernetes:
- Ejecuta el siguiente comando para ver todos los pods, nodos, servicios y deployments creados en el clúster de Kubernetes
```
kubectl get all
```

##  Descripción del Ambiente de Desarrollo y Técnico
El ambiente de desarrollo y técnico para este proyecto se basa en el uso de Google Cloud Platform (GCP) como proveedor de servicios en la nube y Kubernetes como gestor de 
contenedores. A continuación, se detallan los aspectos relevantes de este ambiente:

- **Google Cloud Platform (GCP):** Se utiliza GCP como proveedor de servicios en la nube para desplegar el clúster de Kubernetes y otros servicios necesarios para el proyecto. GCP
ofrece una amplia gama de servicios, como computación, almacenamiento, bases de datos y redes, que se utilizan para construir y desplegar aplicaciones en la nube de manera
escalable y segura.

- **Kubernetes:** Se utiliza Kubernetes como gestor de contenedores para desplegar, escalar y gestionar los contenedores de la aplicación. Kubernetes ofrece características avanzadas
como el balanceo de carga, la auto-reparación y la auto-escalabilidad, que son fundamentales para garantizar la disponibilidad y la escalabilidad de la aplicación.

- **Contenedores Dockerizados:** La aplicación CMS o LMS se empaqueta en contenedores Docker, lo que facilita su despliegue y mantenimiento en el clúster de Kubernetes. Los contenedores
encapsulan el código de la aplicación y todas sus dependencias, lo que garantiza que la aplicación se ejecute de manera consistente en diferentes entornos.


## Aspectos Relevantes
- Utilización de servicios administrados en la nube para mejorar la disponibilidad y escalabilidad de la aplicación.
- Implementación de balanceador de cargas, alta disponibilidad en la capa de aplicación, base de datos y almacenamiento.
- Gestión de certificados SSL y dominios para asegurar la comunicación segura.


## Referencias 
- Video tutorial de Google Cloud Platform sobre Kubernetes: Ver en YouTube (https://www.youtube.com/watch?v=R7FI05Q-nkg)
- Repositorio de ejemplos de Google Cloud Platform para Kubernetes Engine: Ver en GitHub (https://github.com/GoogleCloudPlatform/kubernetes-engine-samples)
- Documentación de Google Cloud sobre el controlador CSI para Cloud Filestore: Ver en Google Cloud (https://cloud.google.com/filestore/docs/csi-driver?hl=es-419#deployment)
- Tutorial de Google Cloud sobre el uso de discos persistentes en Kubernetes Engine: Ver en Google Cloud (https://cloud.google.com/kubernetes-engine/docs/tutorials/persistent-disk?hl=es-419)

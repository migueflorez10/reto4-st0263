# reto4-st0263
Despliegue de la misma aplicación del Reto 3 en un clúster de alta disponibilidad en Kubernetes en AWS o GCP. Utiliza servicios gestionados como EKS o GKE. Asegura balanceo de carga, alta disponibilidad en la aplicación, base de datos y almacenamiento. Implementa HTTPS con un nombre de dominio.


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

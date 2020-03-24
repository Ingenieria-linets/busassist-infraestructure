![GCP SDK](https://www.gstatic.com/devrel-devsite/prod/vf4743e4237527d72f4be8582639e4a529166b52e9bb628e797b1ed38800b278b/cloud/images/cloud-logo.svg)

# [Instalar GCP SDK](https://cloud.google.com/sdk/install)
# [Inicializar SDK](https://cloud.google.com/sdk/docs/initializing?hl=es-419)

```
gcloud init
```
Sigue las instrucciones.

Todos los proyectos estan configurados en la region __us-west1__ y en la zona __us-west1-a__, si la consola no te lo solicita puedes configurarlo manualmente:

```
gcloud config set compute/region us-west1
gcloud config set compute/zone us-west1-a
```

Revisa tu configuración: 
```
gcloud config list
```
_ _ _

# Despliegue BA2
## 1. Crear Cluster en GKE. 
Crea un cluster zonal en __us-west1-a__, el numero de nodos y el tipo de instancia de cada uno de estos, debe ser coherente con la carga de trabajo que recibira.

![](images/gke_cluster.png)
![](images/gke_clusterup.png)

## 2. Crear IPs estaticas.
Es importante que se utilicen Ips estaticas para no  tener que actualizarlas en los DNS en caso de que cambien.

```
gcloud compute addresses create ba2-tutorial-users-ip --region us-west1
gcloud compute addresses create ba2-tutorial-sso-ip --region us-west1
gcloud compute addresses create ba2-tutorial-dispatchapp-ip --region us-west1
gcloud compute addresses create ba2-tutorial-busassist-web-ip --region us-west1
gcloud compute addresses create ba2-tutorial-postgres-ip --region us-west1
gcloud compute addresses create ba2-tutorial-mongodb-users-ip --region us-west1                  
```

Revisa las IPs creadas: 
```
gcloud compute addresses describe ba2-tutorial-users-ip --region us-west1         
gcloud compute addresses describe ba2-tutorial-sso-ip --region us-west1           
gcloud compute addresses describe ba2-tutorial-dispatchapp-ip --region us-west1   
gcloud compute addresses describe ba2-tutorial-busassist-web-ip --region us-west1 
gcloud compute addresses describe ba2-tutorial-postgres-ip --region us-west1      
gcloud compute addresses describe ba2-tutorial-mongodb-users-ip --region us-west1 
```
## 3. Configura los services.yaml
Este paso es igual para los services de todas las apps de ba2. 
![](images/services_setip.png)

## 

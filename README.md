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

## 4. Desencripta los secrets.yaml.enc
Los archivos estan encriptaos con el servicio [KMS](https://cloud.google.com/kms/docs/quickstart). El llavero y las llaves deben estar creadas previamente.

```
gcloud kms keyrings create ba2-tutorial --location global
gcloud kms keys create cluster --location global --keyring ba2-tutorial --purpose encryption
```

Navega al path donde se encuentra el archivo y ejecuta:

```
gcloud kms decrypt --plaintext-file=secret.yaml --ciphertext-file=secret.yaml.enc --location=global --keyring=ba2-tutorial   --key=cluster
```

Para encriptar un archivo:
```
gcloud kms encrypt --plaintext-file=secret.yaml --ciphertext-file=secret.yaml.enc --location=global --keyring=ba2-tutorial   --key=cluster
```

## 5. [Instalar kubectl](https://cloud.google.com/kubernetes-engine/docs/quickstart) 

```
gcloud components install kubectl
```

Verifica la instalación:
```
kubectl version
```
## 6. Conectate al cluster

```
gcloud container clusters get-credentials cluster-tutorial
```

Verifica tu conexión:
```
kubectl config get-contexts
```


## 7. Desplegar Bases de Datos
Antes de desplegar las bases de datos, debes crear el namespace donde se desplegaran.

```
kubectl create namespace databases
```

### 7.1 MongoDB Users
Ubicate en el path __*/mongo-users/__ 

```
kubectl apply -f secret.yaml
kubectl apply -f pv.yaml
kubectl apply -f mongo.yaml
```
Verifica el despliegue:

```
kubectl get deployment -n databases
kubectl get service -n databases
```

Prueba tu conexión: 
El usuario y la contraseña se encuentran en el archivo secrets.yaml, estan codificados en base64, para poder ver los valores usa:

```
echo __stringEnviromentVariablebase64__ | base64 -d
```

CX STRING:
```
mongodb://<user>:<password>@34.82.32.103:27017/admin
```


### 7.2 postgresSQL Users
Ubicate en el path __*/postgresSQL/__

```
kubectl apply -f secret.yaml
kubectl apply -f pv.yaml
kubectl apply -f service.yaml
kubectl apply -f deployment.yaml
```

Verifica el despliegue:

```
kubectl get deployment -n databases
kubectl get service -n databases
```

Prueba tu conexión: 
El usuario y la contraseña se encuentran en el archivo secrets.yaml, estan codificados en base64, para poder ver los valores usa:

```
echo __stringEnviromentVariablebase64__ | base64 -d
```

CX STRING:
```
postgresql://<user>:<password>@35.233.141.24:3365
```





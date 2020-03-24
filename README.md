Pasos: 

1. Crear Ips estaticas para cada aplicación:
   - Las ips deben ser regionales para ser configuradas en un service, cuando se realice el cambio a Ingress, estas ips deben ser de tipo Global.  

gcloud compute addresses create ba2-prod-users-ip --region us-west1                          #ba2-prod-users-ip              --> 35.247.7.178
gcloud compute addresses create ba2-prod-sso-ip --region us-west1                            #ba2-prod-sso-ip                --> 35.199.145.191
gcloud compute addresses create ba2-prod-dispatchapp-ip --region us-west1                    #ba2-prod-dispatchapp-ip        --> 35.233.223.11
gcloud compute addresses create ba2-prod-busassist-web-ip --region us-west1                  #ba2-prod-busassist-web-ip      --> 34.83.161.51
gcloud compute addresses create ba2-prod-postgres-ip --region us-west1                       #ba2-prod-postgres-ip           --> 35.227.138.245
gcloud compute addresses create ba2-prod-mongodb-users-ip --region us-west1                  #ba2-prod-mongodb-users-ip      --> 35.247.100.11


gcloud compute addresses describe ba2-prod-users-ip --region us-west1         
gcloud compute addresses describe ba2-prod-sso-ip --region us-west1           
gcloud compute addresses describe ba2-prod-dispatchapp-ip --region us-west1   
gcloud compute addresses describe ba2-prod-busassist-web-ip --region us-west1 
gcloud compute addresses describe ba2-prod-postgres-ip --region us-west1      
gcloud compute addresses describe ba2-prod-mongodb-users-ip --region us-west1 


1. Deploy postgres: 
    a. pv.yaml
    b. deployment.yaml
    c. service.yaml

2. Deploy mongo users:
    a. pv.yaml
    b. secret.yaml
    c. mongo.yaml

3. Migrate postgress
    a. bundle exec rake db:migrate  -> para migrar 
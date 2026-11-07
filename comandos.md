## Paso 1: Enlazamos nuestra cuenta y proyecto de Google Cloud
gcloud init
 
## Paso 2: Activar la API de Artifact Registry. Generar el Repo
gcloud artifacts repositories create <NOMBRE_DEL_REPOSITORIO> --repository-format docker --project <ID_DEL_PROYECTO> --location us-central1

gcloud artifacts repositories create repositorio-regresion-casas-api --repository-format docker --project datapath-kevin-inofuente --location us-central1

## Paso 3: Construye la Imagen en mi Container y después lo lleva a Artifact Registry Repository
gcloud builds submit \
    --config=cloudbuild.yaml \
    --project=<NOMBRE_PROYECTO> \
    --region=us-central1 \
    --service-account=projects/<NOMBRE_PROYECTO>/serviceAccounts/<EMAIL_DE_TU_SA> \
    --default-buckets-behavior=regional-user-owned-bucket

gcloud builds submit \
    --config=cloudbuild.yaml \
    --project=datapath-kevin-inofuente \
    --region=us-central1 \
    --service-account=projects/datapath-kevin-inofuente/serviceAccounts/<EMAIL_DE_TU_SA> \
    --default-buckets-behavior=regional-user-owned-bucket

## Paso 4: Colocamos en servicio la aplicación
gcloud run services replace service.yaml --region us-central1 --project <ID_DEL_PROYECTO>

gcloud run services replace service.yaml --region us-central1 --project datapath-kevin-inofuente


## Paso 5: Se ejecuta una sola vez para modificar los permisos (IAM)
gcloud run services set-iam-policy <NOMBRE_DEL_SERVICIO_EN_service.yaml> gcr-service-policy.yaml --region us-central1 --project <ID_DEL_PROYECTO>

gcloud run services set-iam-policy servicio-api-proyecto-regresion-kevin-inofuente gcr-service-policy.yaml --region us-central1 --project datapath-kevin-inofuente
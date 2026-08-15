## Paso 1: Enlazamos nuestra cuenta y proyecto de Google Cloud
gcloud init
 
## Paso 2: Activar la API de Artifact Registry. Generar el Repo
gcloud artifacts repositories create <NOMBRE_DEL_REPOSITORIO> --repository-format docker --project <ID_DEL_PROYECTO> --location us-central1

gcloud artifacts repositories create repositorio-regresion-casas-api --repository-format docker --project datapath-mlops-kevin-inofuente --location us-central1


# ========================= PASOS OPCIONALES PARA CUENTAS ANTIGUAS ===============================
## Pasos únicos (nuevo) 3 y 4 - Solo se hace una vez por proyecto ---------------------------------------------------
## Paso 3: Obtener el project number
  gcloud projects describe <ID_DEL_PROYECTO> --format="value(projectNumber)"
  gcloud projects describe datapath-kevin-inofuente --format="value(projectNumber)"

## Paso 4: Dar permiso al service account de Cloud Build (reemplaza <NUMERO_DEL_PROYECTO>)
  gcloud projects add-iam-policy-binding <ID_DEL_PROYECTO> --member="serviceAccount:<NUMERO_DEL_PROYECTO>@cloudbuild.gserviceaccount.com" --role="roles/iam.serviceAccountUser"  
  gcloud projects add-iam-policy-binding datapath-kevin-inofuente --member="serviceAccount:967741749170@cloudbuild.gserviceaccount.com" --role="roles/iam.serviceAccountUser"  
# =================================================================================================


## Paso 3: Construye la Imagen en mi Container y después lo lleva a Artifact Registry Repository
### nombre proyecto / nombre repo / nombre imagen
gcloud builds submit --config=cloudbuild.yaml --project <ID_DEL_PROYECTO> --region us-central1

gcloud builds submit --config=cloudbuild.yaml --project datapath-mlops-kevin-inofuente --region us-central1

## Paso 4: Colocamos en servicio la aplicación
gcloud run services replace service.yaml --region us-central1 --project <ID_DEL_PROYECTO>

gcloud run services replace service.yaml --region us-central1 --project datapath-mlops-kevin-inofuente


## Paso 5: Se ejecuta una sola vez para modificar los permisos (IAM)
gcloud run services set-iam-policy <NOMBRE_DEL_SERVICIO_EN_service.yaml> gcr-service-policy.yaml --region us-central1 --project <ID_DEL_PROYECTO>

gcloud run services set-iam-policy servicio-api-backend-kevao-inofuente gcr-service-policy.yaml --region us-central1 --project datapath-mlops-kevin-inofuente
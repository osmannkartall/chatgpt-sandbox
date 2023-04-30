# Deploying and Testing a Public Docker Image with Google Cloud Run

Here's a step-by-step guide for Deploying and Testing a Public Docker Image with Google Cloud Run:

1. Authenticate and set the default project:
```
gcloud auth login
gcloud config set project [PROJECT_ID]
```

2. Create a new service account:
```
gcloud iam service-accounts create [SERVICE_ACCOUNT_NAME] --display-name [SERVICE_ACCOUNT_DISPLAY_NAME]
```

3. Grant the Cloud Run Invoker role to the service account:
```
gcloud projects add-iam-policy-binding [PROJECT_ID] --member="serviceAccount:[SERVICE_ACCOUNT_NAME]@[PROJECT_ID].iam.gserviceaccount.com" --role="roles/run.invoker"
```

4. Create a new Cloud Run service:
```
gcloud run deploy [SERVICE_NAME] --image [DOCKER_IMAGE] --allow-unauthenticated --platform managed
```

5. Test the Cloud Run service:
```
curl -H "Authorization: Bearer $(gcloud auth print-identity-token)" $(gcloud run services describe [SERVICE_NAME] --platform managed --format "value(status.url)")
```
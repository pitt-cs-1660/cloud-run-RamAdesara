REGION = us-central1
PROJECT_ID = assignment-3-3-26-2025-6-27pm

# 1
gcloud builds submit --region us-central1 --tag us-central1-docker.pkg.dev/assignment-3-3-26-2025-6-27pm/tabs-vs-spaces/app:v1

# 2
gcloud artifacts repositories add-iam-policy-binding tabs-vs-spaces \
  --location=us-central1 \
  --member="principalSet://iam.googleapis.com/${WORKLOAD_ID_POOL}/attribute.repository/" \
  --role="roles/artifactregistry.createOnPushWriter" \
  --project="assignment-3-3-26-2025-6-27pm"

# 3
gcloud projects add-iam-policy-binding assignment-3-3-26-2025-6-27pm \
  --member=serviceAccount:tabs-vs-spaces@assignment-3-3-26-2025-6-27pm.iam.gserviceaccount.com \
  --role=roles/datastore.owner

# 4
gcloud run deploy tabs-vs-spaces \
  --allow-unauthenticated \
  --image us-central1-docker.pkg.dev/assignment-3-3-26-2025-6-27pm/tabs-vs-spaces/app:v1 \
  --service-account tabs-vs-spaces@assignment-3-3-26-2025-6-27pm.iam.gserviceaccount.com \
  --region us-central1 \
  --port 8000

# 5
gcloud run services logs read tabs-vs-spaces --region us-central1

CI/CD Pipeline on GCP
# Create a service account in the project
cd infrastructure 

SERVICE_ACCOUNT_NAME=pipeline

gcloud config set core/project $PROJECT_ID

gcloud iam service-accounts create $SERVICE_ACCOUNT_NAME

# Grant a project owner IAM role to the service account
gcloud projects add-iam-policy-binding $PROJECT_ID --member serviceAccount:$SERVICE_ACCOUNT_NAME@$PROJECT_ID.iam.gserviceaccount.com --role roles/owner

# Create and download the access key for the service account
gcloud iam service-accounts keys create access.json --iam-account=$SERVICE_ACCOUNT_NAME@$PROJECT_ID.iam.gserviceaccount.com

# To Deploy the CI/CD Pipeline
terraform init

terraform plan ( NB: Ensure to drag access.json into 'infrastructure' directory )

terraform apply -auto-approve

# CI/CD in Action
cd service

git init

git add -A

git commit -m "initial commit"

git config --global credential.https://source.developers.google.com.helper gcloud.sh

git remote add google <value from urls.repo attribute from outputs.tf previously>
  
gcloud auth login
  
git push --all google
  
Go to >> Cloud Build >> Triggers >> RUN 
 
After build is complete, check build history and click the SERVICE url


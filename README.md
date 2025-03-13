# Tiny AI Platform

A tiny AI platform that is suitable to run locally on RD, k3s, Kind et.al

## Setup

```bash
# install required dependencies via Brewfile
brew bundle

# Alternatively, install at least Taskfile (manually)
brew install go-task

# make sure you have set a valid GITHUB_TOKEN environment variable
# make sure you have set the correct kube-context, e.g. rancher-desktop
task create-secret
task flux-bootstrap

# the Kube Prometheus Stack is accessiable via Ingress
open http://grafana.127.0.0.1.sslip.io

# to test the OpenAI proxy, issue the following curl command
curl http://openai.127.0.0.1.sslip.io/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{
     "model": "gpt-4o-mini",
     "messages": [{"role": "user", "content": "Say this is a test!"}],
     "temperature": 0.7
   }'

# the Langflow UI is accessible via Ingress
open http://langflow.127.0.0.1.sslip.io 

# the Jupyther Hub UI is accessible via Ingress
open http://jupyther.127.0.0.1.sslip.io 
```

## GCP with Crossplane (optional)

```bash
# create a GCP service account with the required roles, e.g. storage admin
export PROJECT_ID=cloud-native-experience-lab
gcloud iam service-accounts create crossplane-system --display-name=Crossplane
gcloud projects add-iam-policy-binding $PROJECT_ID --role=roles/iam.serviceAccountUser --member serviceAccount:crossplane-system@$PROJECT_ID.iam.gserviceaccount.com
gcloud projects add-iam-policy-binding $PROJECT_ID --role=roles/storage.admin --member serviceAccount:crossplane-system@$PROJECT_ID.iam.gserviceaccount.com
gcloud iam service-accounts keys create $HOME/.gcp/crossplane-system.json --iam-account crossplane-system@$PROJECT_ID.iam.gserviceaccount.com

# create Kubernetes secret and apply provider configuration
kubectl create secret generic gcp-secret -n crossplane-system --from-file=credentials=${HOME}/.gcp/crossplane-system.json
kubectl apply -k platform/cloud/gcp/
```

## AWS with Crossplane (optional)

```bash
# create Kubernetes secret and apply provider configuration
kubectl create secret generic aws-secret -n crossplane-system --from-file=credentials=${HOME}/.aws/credentials
kubectl apply -k platform/cloud/aws/
```

## Maintainer

M.-Leander Reimer (@lreimer), <mario-leander.reimer@qaware.de>

## License

This software is provided under the MIT open source license, read the `LICENSE`
file for details.

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
task bootstrap-flux

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

## Crossplane GCP

```bash
gcloud iam service-accounts create crossplane-system --display-name=Crossplane
gcloud projects add-iam-policy-binding cloud-native-experience-lab --role=roles/iam.serviceAccountUser --member serviceAccount:crossplane-system@cloud-native-experience-lab.iam.gserviceaccount.com
gcloud projects add-iam-policy-binding cloud-native-experience-lab --role=roles/storage.admin --member serviceAccount:crossplane-system@cloud-native-experience-lab.iam.gserviceaccount.com
gcloud iam service-accounts keys create $HOME./.gcp/credentials.json --iam-account crossplane-system@cloud-native-experience-lab.iam.gserviceaccount.com
```

## Maintainer

M.-Leander Reimer (@lreimer), <mario-leander.reimer@qaware.de>

## License

This software is provided under the MIT open source license, read the `LICENSE`
file for details.

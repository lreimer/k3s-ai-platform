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
taks create-secret
task bootstrap-flux

# several dashboards and services are accessible via Ingress
open http://grafana.127.0.0.1.sslip.io

```

## Maintainer

M.-Leander Reimer (@lreimer), <mario-leander.reimer@qaware.de>

## License

This software is provided under the MIT open source license, read the `LICENSE`
file for details.

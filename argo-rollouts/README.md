Controller Installation:
------------------------

kubectl create namespace argo-rollouts
kubectl apply -n argo-rollouts -f https://github.com/argoproj/argo-rollouts/releases/latest/download/install.yaml


Kubectl Plugin Installation:
----------------------------

# Install Argo Rollouts Kubectl plugin with curl.

curl -LO https://github.com/argoproj/argo-rollouts/releases/latest/download/kubectl-argo-rollouts-linux-amd64

# Make the kubectl-argo-rollouts binary executable.

chmod +x ./kubectl-argo-rollouts-linux-amd64

# Move the binary into your PATH.

sudo mv ./kubectl-argo-rollouts-linux-amd64 /usr/local/bin/kubectl-argo-rollouts

# Test to ensure the version you installed is up-to-date:

kubectl argo rollouts version

# Move to the project repo for rollout manifests related to the application
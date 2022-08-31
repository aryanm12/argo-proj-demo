Installation:
--------------

kubectl create namespace argo
kubectl apply -n argo -f https://github.com/argoproj/argo-workflows/releases/download/v3.3.9/install.yaml


Patch argo-server authentication:
----------------------------------

kubectl patch deployment \
  argo-server \
  --namespace argo \
  --type='json' \
  -p='[{"op": "replace", "path": "/spec/template/spec/containers/0/args", "value": [
  "server",
  "--auth-mode=server"
]}]'


Patch argo-server service to run on NodePort:
----------------------------------------------

kubectl patch svc argo-server -n argo -p '{"spec": {"type": "NodePort"}}'


Install argo CLI:
------------------

# Download the binary
curl -sLO https://github.com/argoproj/argo-workflows/releases/download/v3.3.9/argo-linux-amd64.gz

# Unzip
gunzip argo-linux-amd64.gz

# Make binary executable
chmod +x argo-linux-amd64

# Move binary to path
mv ./argo-linux-amd64 /usr/local/bin/argo

# Test installation
argo version


# Create Kubernetes Secrets for running publishing Docker Image:
----------------------------------------------------------------

# Publishing images requires an access token. For hub.docker.com you can create one at https://hub.docker.com/settings/security
# This needs to be mounted as `$DOCKER_CONFIG/config.json`. To do this, you'll need to create a secret as follows:

export DOCKER_USERNAME=aryanmohan
export DOCKER_TOKEN=dckr_pat_R4B6AbK6MK-OM6ngtND7x5vs8iE
kubectl create secret docker-registry regcred --from-literal="config.json={\"auths\": {\"https://index.docker.io/v1/\": {\"auth\": \"$(echo -n $DOCKER_USERNAME:$DOCKER_TOKEN|base64)\"}}}" -n argo


# Create Cluster workflow template for container build
-------------------------------------------------------

argo cluster-template create -n argo argo-workflow/workflow-templates/container-image.yaml
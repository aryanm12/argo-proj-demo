Install Argo CD:
----------------

kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml


Download and Install argocd cli:
--------------------------------

curl -sSL -o /usr/local/bin/argocd https://github.com/argoproj/argo-cd/releases/latest/download/argocd-linux-amd64
chmod +x /usr/local/bin/argocd


Access The Argo CD API Server:
------------------------------

Change the argocd-server service type to NodePort:

kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "NodePort"}}'


Login Using The CLI:
--------------------

argoPass=$(kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo)

nodePort=$(kubectl get svc -n argocd argocd-server -o jsonpath="{.spec.ports[1].nodePort}")

argocd login --insecure localhost:$nodePort --username admin --password $argoPass
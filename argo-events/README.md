Installation:
--------------

Create the namespace:
----------------------

kubectl create namespace argo-events

Deploy Argo Events, SA, ClusterRoles, Sensor Controller, EventBus Controller and EventSource Controller:
---------------------------------------------------------------------------------------------


kubectl apply -f https://raw.githubusercontent.com/argoproj/argo-events/stable/manifests/install.yaml
# Install with a validating admission controller
kubectl apply -f https://raw.githubusercontent.com/argoproj/argo-events/stable/manifests/install-validating-webhook.yaml


Deploy the eventbus:
--------------------
kubectl apply -n argo-events -f https://raw.githubusercontent.com/argoproj/argo-events/stable/examples/eventbus/native.yaml


Deploy the default event source:
--------------------------------

kubectl apply -n argo-events -f webhook-dotnet.yaml
kubectl apply -n argo-events -f webhook-java.yaml


Patch the webhook service:
---------------------------

kubectl patch svc webhook-java-eventsource-svc -n argo-events -p '{"spec": {"
type": "NodePort"}}'

kubectl patch svc webhook-dotnet-eventsource-svc -n argo-events -p '{"spec":
{"type": "NodePort"}}'


Create a service account with RBAC settings to allow the sensor to trigger workflows, and allow workflows to function.
-----------------------------------------------------------------------------------

 # sensor rbac
kubectl apply -n argo-events -f https://raw.githubusercontent.com/argoproj/argo-events/master/examples/rbac/sensor-rbac.yaml
 # workflow rbac
kubectl apply -n argo-events -f https://raw.githubusercontent.com/argoproj/argo-events/master/examples/rbac/workflow-rbac.yaml


# To trigger a specific workflow create a sensor in the project specific repo e.g: dotnet-argo-demo

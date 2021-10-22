# Argo CD automation starter

## Pre-requisites

* Kubernetes cluster

* [Argo CD CLI](https://argo-cd.readthedocs.io/en/stable/getting_started/#2-download-argo-cd-cli)

## Getting Started

1. Install Argo CD

    ```Shell
    kubectl create namespace argocd
    kubectl apply -n argocd -f argo-cd/install.yaml
    kubectl --namespace argocd rollout status deployment argocd-server

    # Access The Argo CD API Server
    kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'
    kubectl port-forward svc/argocd-server -n argocd 8080:443 > /dev/null 2>&1 &

    # Login Using The CLI
    PWD=$( kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d )
    argocd login localhost:8080 --username admin --password $PWD --insecure

    # Change initial admin password and remove secret from cluster
    argocd account update-password --current-password $PWD --new-password ChangeMe --insecure
    kubectl -n argocd delete secret argocd-initial-admin-secret
    ```

## Argo CD Automation

1. Configure Argo CD Production application and project

    ```Shell
    kubectl apply -f project.yaml
    kubectl apply -f apps.yaml
    ```

TODO ...

<!--

1. Install NGinx Ingress

    ```Shell
    kubectl create namespace nginx-ingress
    helm install my-release nginx-stable/nginx-ingress --namespace nginx-ingress
    kubectl patch svc my-release-nginx-ingress --namespace nginx-ingress -p '{"spec": {"type": "NodePort"}}'

    SERVICE_IP=$(kubectl get nodes -o wide -o jsonpath='{.items[].status.addresses[].address}')
    SERVICE_PORT=$(kubectl get svc --namespace nginx-ingress my-release-nginx-ingress -o jsonpath='{.spec.ports[].nodePort}')

    echo "http://${SERVICE_IP}:${SERVICE_PORT}"
    ```

1. Install Sample Web application

    ```Shell
    helm install sample-nginx-release bitnami/nginx
    kubectl patch svc sample-nginx-release --namespace default -p '{"spec": {"type": "NodePort"}}'

    SERVICE_IP=$(kubectl get nodes -o wide -o jsonpath='{.items[].status.addresses[].address}')
    SERVICE_PORT=$(kubectl get svc --namespace default sample-nginx-release -o jsonpath='{.spec.ports[].nodePort}')

    echo "http://${SERVICE_IP}:${SERVICE_PORT}"
    ```

1. Clean-up

    ```Shell
    helm uninstall sample-nginx-release --namespace default
    kubectl delete namespace nginx-ingress
    ``` -->

## References

* [Nginx Ingress Controller installation with Helm](https://docs.nginx.com/nginx-ingress-controller/installation/installation-with-helm/)

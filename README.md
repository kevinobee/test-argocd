
# NGinx Ingress for Kubernetes

> https://docs.nginx.com/nginx-ingress-controller/installation/installation-with-helm/

* Pre-Requisites:

    ```Shell
    # Start a cluster with Kind
    kind cluster create
    ```

* Install NGinx Ingress:

    ```Shell
    kubectl create namespace nginx-ingress
    helm install my-release nginx-stable/nginx-ingress --namespace nginx-ingress
    kubectl patch svc my-release-nginx-ingress --namespace nginx-ingress -p '{"spec": {"type": "NodePort"}}'

    SERVICE_IP=$(kubectl get nodes -o wide -o jsonpath='{.items[].status.addresses[].address}')
    SERVICE_PORT=$(kubectl get svc --namespace nginx-ingress my-release-nginx-ingress -o jsonpath='{.spec.ports[].nodePort}')

    echo "http://${SERVICE_IP}:${SERVICE_PORT}"
    ```

* Install Sample Web application:

    ```Shell
    helm install sample-nginx-release bitnami/nginx
    kubectl patch svc sample-nginx-release --namespace default -p '{"spec": {"type": "NodePort"}}'

    SERVICE_IP=$(kubectl get nodes -o wide -o jsonpath='{.items[].status.addresses[].address}')
    SERVICE_PORT=$(kubectl get svc --namespace default sample-nginx-release -o jsonpath='{.spec.ports[].nodePort}')

    echo "http://${SERVICE_IP}:${SERVICE_PORT}"
    ```

* Clean-up:

    ```Shell
    helm uninstall sample-nginx-release --namespace default
    kubectl delete namespace nginx-ingress
    ```

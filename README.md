
# NGinx Ingress for Kubernetes

> https://docs.nginx.com/nginx-ingress-controller/installation/installation-with-helm/

Install NGinx Ingress:

```Shell
helm install my-release nginx-stable/nginx-ingress
kubectl delete svc my-release-nginx-ingress
kubectl expose deployment my-release-nginx-ingress --type NodePort --port=80
```

Uninstall NGinx Ingress:

```Shell
helm uninstall my-release
```

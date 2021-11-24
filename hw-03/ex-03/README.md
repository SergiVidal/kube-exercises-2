# Questions

## 1. [Horizontal Pod Autoscaler] Crea un objeto de kubernetes HPA, que escale a partir de las métricas CPU o memoria (a vuestra elección). Establece el umbral al 50% de CPU/memoria utilizada, cuando pase el umbral, automáticamente se deberá escalar al doble de replicas.

```console
kubectl create -f deploy.yaml
kubectl create -f svc.yaml
kubectl create -f hpa.yaml

kubectl run -i --tty load-generator --rm --image=busybox --restart=Never --
/bin/sh -c "while sleep 0.01; do wget -q -O- http://svc-hw03; done"
```

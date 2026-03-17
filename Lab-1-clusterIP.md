## Step 1: Deploy a simple app

```
kubectl create deployment web --image-nginx --replicas=2
```

## Step 2: Expose as ClusterIP

```
kubectl expose deployment web --name web-svc --port=80 --type ClusterIP 
kubectl get svc
```

## Step 3: Access using busybox pod

```
kubectl run test --image-busybox -it --rm -- sh
wget -qo- http://web
```

## Step 4: Clean up

```
kubectl delete deploy web
kubectl delete svc web-svc
kubectl delete pod test
```
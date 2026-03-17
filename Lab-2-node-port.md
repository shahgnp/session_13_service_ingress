## Step 1: Deploy a simple app

```
kubectl create deployment web --image nginx --replicas=2
```

## Step 2: Expose as NodePort

```
kubectl expose deployment web --name web-svc --port=80 --type NodePort 
kubectl get svc
```

## Step 3: Access externally

```
curl http://<NodeIP>:<NodePort>
```

## Step 4: Clean up

```
kubectl delete deploy web
kubectl delete svc web-svc
```
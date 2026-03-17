## Step 1: Deploy a simple app

```
kubectl create deployment web --image nginx --replicas=2
```

## Step 2: Expose as LoadBalancer

```
kubectl expose deployment web --name web-svc --port=80 --type LoadBalancer 
kubectl get svc
```

## Step 3: Access externally

```
curl http://<loadbalancer-IP>
```

## Step 4: Clean up

```
kubectl delete deploy web
kubectl delete svc web-svc
```
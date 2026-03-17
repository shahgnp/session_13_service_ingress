## Step 1: Create and expose services

### Deploy Backend (API)

```bash
kubectl create deployment backend-app --image=traefik/whoami
kubectl expose deployment backend-app --port=80 --name=backend-svc
```

### Deploy Frontend (Web)

```bash
kubectl create deployment frontend-app --image=nginx
kubectl expose deployment frontend-app --port=80 --name=frontend-svc
```

## Step 2: Create istio gateway

```yaml
apiVersion: networking.istio.io/v1
kind: Gateway
metadata:
  name: shared-gateway
  namespace: istio-ingress
spec:
  selector:
    istio: ingress # Matches your central proxy
  servers:
  - port:
      number: 80
      name: http
      protocol: HTTP
    hosts:
    - "*.demo.local" # Wildcard allows all participant names
```
*Only one gateway is required. The gateway uses  wildcard domain name that allows the domains within the wildcard vicinity. Instructor will do this for you*

## Step 3: Create a virtual service

The Cluster Admin has already created a central `Gateway` listening for `*.demo.local`. We just need to bind our routing rules to it.

Create `virtualservice.yaml` (Replace `<your-name>`):
```yaml
apiVersion: networking.istio.io/v1
kind: VirtualService
metadata:
  name: app-routing
spec:
  hosts:
  - "<your-name>.demo.local"
  gateways:
  - istio-ingress/shared-gateway   # Format is namespace/gateway-name
  http:
  - match:
    - uri:
        prefix: /api     
    rewrite:
      uri: /             
    route:
    - destination:
        host: backend-svc
        port:
          number: 80
  - match:
    - uri:
        prefix: /        
    route:
    - destination:
        host: frontend-svc
        port:
          number: 80
```

## Step 4: Apply and Verify

```
kubectl apply -f virtualservice.yaml
```
### Add it to your /etc/hosts
```
<ISTIO_IP> username.demo.local
```

*You may not have the permission to edit /etc/hosts*

### Curl the output

```
curl http://username.demo.local/       # Should return NGINX HTML (Frontend)
curl http://username.demo.local/api       # Should return WHOAMI (Backend)
```
or

```
# Test Frontend
curl -H "Host: <username>.demo.local" http://10.0.60.203/

# Test Backend API
curl -H "Host: <username>.demo.local" http://10.0.60.203/api
```
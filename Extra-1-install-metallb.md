## Prerequisites
- Helm binary installed

## Step 1: Create metallb-system namespace

```
k create ns metallb-system
```

## Step 2: Add metallb repo

```
helm repo add metallb https://metallb.github.io/metallb
helm repo update
helm repo list
```

## Step 3: Install metallb

```
helm -n metallb-system install metallb metallb/metallb
k get all -n metallb-system
```

## Step 4: Create ipaddresspool and l2advertisement yaml files

filename: first-pool.yaml
```
---
apiVersion: metallb.io/v1beta1
kind: IPAddressPool
metadata:
  name: "first-pool"
  namespace: metallb-system
spec:
  addresses:
    - "10.0.60.203-10.0.60.217"
```

filename: l2-config.yaml
```
---
apiVersion: metallb.io/v1beta1
kind: L2Advertisement
metadata:
  name: "first-pool-l2"
  namespace: metallb-system
spec:
  ipAddressPools:
    - "first-pool"
```

Apply

```
k apply -f first-pool.yaml
k apply -f l2-config.yaml
```
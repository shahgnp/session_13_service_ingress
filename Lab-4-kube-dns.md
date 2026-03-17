# Lab 3: Service Discovery via DNS

## Step 1: Deploying the service
Create a web pod and expose it in your respective namespace

```
k run web --image nginx -n <namespace>
k expose pod web --name web-svc --port 80 -n <namespace>
```

## Step 2: Looking up for the service

Run a test pod
```
k run test --image nginx -n <namespace>
```

Exec into test pod

```
k -n <namespace> exec -it test -- /bin/sh
```

Fetch the web pod
```
nslookup web # this might fail
nslookup web.<namespace>.svc.cluster.local # more generic
```

## Step 3: Lookup a service from default namespace

Create a service in the default namespace

```
k run web-default --image nginx -n default
k expose pod web-default --name web-default --port 80 -n default
```

Fetch the web-default pod
```
nslookup web-default # this might fail
nslookup web.default.svc.cluster.local # more generic
```

## Step 4: Clean up

```
k -n <namespace> delete po web
k -n <namespace> delete po test
k -n <namespace> delete svc web-svc
```
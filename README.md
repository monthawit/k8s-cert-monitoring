# k8s-cert-monitoring
k8s-cert-monitoring

## Create secret for authen

### Option 1 : username & pass ( use this ! )
```bash
kubectl create secret generic basic-auth \
  --from-literal=username=admin \
  --from-literal=password=admin123 \
  -n your-namespace
```

### option 2 : htpasswd
```bash
htpasswd -c auth admin

ubectl create secret generic basic-auth \
  --from-file=auth \
  -n your-namespace
```

## how to confi prometheus-prometheus.yaml 
```yaml
  remoteWrite:
  - url: https://mimir-gateway.metrics.olsxops.com/api/v1/push
    name: kube-cluster-01
    basicAuth:
      username:
        name: secret-name
        key: username
      password:
        name: secret-name
        key: password
    headers:
      "X-Scope-OrgID": kubly-dev
    writeRelabelConfigs:            
    - targetLabel: cluster_name
      replacement: 'kube-cluster-01'
  - url: https://mimir-gateway.metrics.olsxops.com/api/v1/push
    name: for-mon-cert-name
    basicAuth:
      username:
        name: secret-name
        key: username
      password:
        name: secret-name
        key: password
    headers:
      "X-Scope-OrgID": for-mon-cert-name
    writeRelabelConfigs:
    - sourceLabels: [__name__]
      regex: "x509_.*"
      action: keep      
    - targetLabel: cluster
      replacement: kube-cluster-01

```

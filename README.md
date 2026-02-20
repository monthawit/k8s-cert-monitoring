# k8s-cert-monitoring
k8s-cert-monitoring

## how to confi prometheus-prometheus.yaml 
```yaml
  remoteWrite:
  - url: https://mimir-gateway.metrics.olsxops.com/api/v1/push
    name: secret-name
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
    name: for-cert-name
    basicAuth:
      username:
        name: secret-name
        key: username
      password:
        name: secret-name
        key: password
    headers:
      "X-Scope-OrgID": for-cert-name
    writeRelabelConfigs:
    - sourceLabels: [__name__]
      regex: "x509_.*"
      action: keep      
    - targetLabel: cluster
      replacement: kubly-dev 

```

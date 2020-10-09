# Add Request Headers with a virtual service

One is able to replace or add a Header via a virtual service by adding a headers field to the spec block of a virtual service.

```yaml
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
  name: reviews-route
spec:
  hosts:
  - reviews.prod.svc.cluster.local
  http:
  - headers:
      request:
        set:
          test: true
```

[Documentation](https://istio.io/latest/docs/reference/config/networking/virtual-service/#Headers)

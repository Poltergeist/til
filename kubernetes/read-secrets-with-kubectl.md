# Read a Secret from kubernetes

You can use the following command to read a secret from kubernetes via the kubectl
```sh
kubectl get secrets -n <% namespace %> <% secret_name %> -o jsonpath='{.data.<% secret key %>}' | base64 -d
```

One is grabbing the secret object from kuberenetes and then deep diving into the object to fetch one secret which is bas64 encoded and then piped into base64.

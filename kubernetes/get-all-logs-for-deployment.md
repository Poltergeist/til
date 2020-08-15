# Get all logs for a deployment in Kubernetes

It is possible to get logs for all containers in a pod and therefore also get all the logs of all the pods from a single deployment. The trick is to use the common label for the deployment e.g.:

```
kubectl logs -l app=<APP NAME> --all-containers --max-log-requests=<NUMBER OF CONTAINERS> -n <NAMESPACE>
```

It is also possible to follow these logs by adding the `-f` arg.
```
kubectl logs -l app=<APP NAME> --all-containers -f --max-log-requests=<NUMBER OF CONTAINERS> -n <NAMESPACE> -f
```


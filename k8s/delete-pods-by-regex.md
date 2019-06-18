# Delete Pods by Regex

```sh
kubectl get pods -n mynamespace --no-headers=true | awk '/pattern/{print $1}'| xargs  kubectl delete -n mynamespace pod
```

Source
https://medium.com/faun/delete-kubernetes-pods-with-a-regex-f396291bba0e



Modify the `skaffold.yaml` file to specify the port to forward to.  This is optional, Skaffold will allocate a random port. but for the sake of everyone running through this workshop and having
some consistency we specify the port in our Skaffold configuration.

```editor:insert-lines-before-line
file: ~/demo/skaffold.yaml
line: 23
text: |
      - resourceType: service
        resourceName: k8s-workshop-name-service 
        port: 80
        localPort: 4504
```
Deploy the apps using Skaffold

```execute-1
skaffold dev --port-forward

```



*   This should deploy both the k8s-demo-app and the name-service app


```execute-2
kubectl get all
```

Your output will be similar to:
```bsh
NAME                                             READY   STATUS    RESTARTS   AGE
pod/k8s-demo-app-5b957cf66d-w7r9d                1/1     Running   0          172m
pod/k8s-workshop-name-service-79475f456d-4lqgl   1/1     Running   0          173m

NAME                                TYPE           CLUSTER-IP       EXTERNAL-IP     PORT(S)        AGE
service/k8s-demo-app                LoadBalancer   10.100.200.102   35.238.231.79   80:30068/TCP   173m
service/k8s-workshop-name-service   ClusterIP      10.100.200.16    <none>          80/TCP         173m

NAME                                        READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/k8s-demo-app                1/1     1            1           173m
deployment.apps/k8s-workshop-name-service   1/1     1            1           173m

NAME                                                   DESIRED   CURRENT   READY   AGE
replicaset.apps/k8s-demo-app-5b957cf66d                1         1         1       172m
replicaset.apps/k8s-demo-app-fd497cdfd                 0         0         0       173m
replicaset.apps/k8s-workshop-name-service-79475f456d   1         1         1       173m

```



Because we deployed two services and supplied the `-port-forward` flag, Skaffold will forward two ports

```
Port forwarding service/k8s-demo-app in namespace lab-workshop-1-w01-s002, remote port 80 -> address 127.0.0.1 port 4503
Port forwarding service/k8s-workshop-name-service in namespace lab-workshop-1-w01-s002, remote port 80 -> address 127.0.0.1 port 4504

```


*   Test the name service


```execute-2
curl localhost:4504
```


Hitting the service multiple times will return a different name



*   Test the k8s-demo-app which should now make a request to the name-service


```execute-2
curl localhost:4503
```


Making multiple requests should result in different names coming from the name-service
```execute-2
curl localhost:4503
```


Stop the Skaffold process to clean everything up before moving to the next step
```terminal:interrupt-all
```


---




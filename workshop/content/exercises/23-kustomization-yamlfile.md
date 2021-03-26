
### 
**kustomization.yaml**



*   In `kustomize/base` create a new file called `kustomization.yaml` and add the following to it.

```editor:append-lines-to-file
file: ~/demo/kustomize/base/kustomization.yaml
text: |
        apiVersion: kustomize.config.k8s.io/v1beta1
        kind: Kustomization

        resources:	
        - service.yaml
        - deployment.yaml
        - ingress.yaml
```


NOTE: Optionally, you can now remove all the labels and annotations in the metadata of objects and specs inside objects. Kustomize adds default values that link objects to each other (e.g. to link a service to a deployment). If there is only one of each in your manifest then it will pick something sensible.

---


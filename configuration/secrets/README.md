# Secrets

Video Link:
[Secrets](https://www.youtube.com/watch?v=MTnQW9MxnRI)

## Secrets Store CSI Driver

1. Why was this tool ultimately created?
2. How does this tool differ from others like external secrets operator and sealed secrets?

### Why?

* The secrets are not secure. The values inside it are just base64 encoded string. Any one who has access to the namespace as well has access to these secrets. So any one can get the secret value and the credentials

* Most of the organizations have adopted to the new secrets store
    - Hashicorp Valut
    - AWS secrets Manager
    - Azure key vault
    - ...

* We need a way in which kubernetes would fetch the secrets from those mangers and have the value in the secrets. 
* Hence there are tools to do the sync between the exteranl secret manager and the kubernetes secrets

### Secret Store CSI Driver

* It synchronizes secrets from exteranl APIs and mounts them into containers as volumes
* Allows you to manage secres in a central place like Hashicorp valut or AWS secrets manager
* We don't need to check secrets in GIT
* No longer store credentials in kuberentes secrets
* If we use something like External secret operator or sealed secrets, you are going to grab the secrets from central secret store and you are going to sync it with kubernetes secret.
* With CSI driver, you don't actually create a kuberentes secret
* It is going to pull the secrets dynamically from the central secret manager

#### Advantages
* Minimizes the attac surface as much as possible

### Example
Let us consider we need db credentials store in AWS secrets manager. The configuration would be something like this

```yaml
apiVersion: secrets-store.csi.x-k8s.io/v1
kind: SecretsProviderClass
metadata: 
    name: db-secrets
spec:
    provider: aws
    parameters:
        objects: |
            - objectName: "db-creds"
              objectType: "secretsmanager"
```

Since a pod requires these details, we update pod volume with this information.

```yaml
spec:
    volumes:
        - name: db-creds
          csi:
            driver: secrets-store.csi.k8s.io
            readOnly: true
            volumeAttributes:
                secretProviderClass: db-secrets
    containers:
    - name: nginx
      image: nginx
      volumeMounts:
        - name: db-creds
          mountPath: /tmp
```

* When a pod gets created, the kubelet knows we need the information from the pod definition and talks to CSI Driver. CSI Driver then points to Secrets provider and the secret provider will talk to AWS Secret Manager and the data flows back to the CSI Driver.
* CSI Driver will mount the volume inside the pod and the volume would contain the secrets.

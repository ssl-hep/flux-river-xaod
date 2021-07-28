# flux-river-xaod
Flux Configuration for xAOD ServiceX on River.  This instnatiates a ServiceX
instance in the servicex-testing-1 namespace on the river cluster. 


## Creating sealed secrets

To create a sealed secret, do the following:

* Generate a sealed secret yaml file similar to below:

```
apiVersion: v1
data:
  minio-accesskey: base64 encoded secret
  minio-secretkey: base64 encoded secret
  rabbitmq-password: base64 encoded secret
  rabbitmq-erlang-cookie: base64 encoded secret
  postgresql-postgres-password: base64 encoded secret
kind: Secret
metadata:
  annotations:
    sealedsecrets.bitnami.com/managed: "true"
    sealedsecrets.bitnami.com/namespace-wide: "true"  
  name: servicex-secrets
  namespace: servicex-testing-1
type: Opaque

``` 
* Run `kubeseal  --format yaml -n kube-system --controller-name=sealed-secrets < secrets.yaml  > sealed-secrets.yaml`
* Copy the `sealed-secrets.yaml` file to the flux configuration directory
  reference it in the kustomize.yaml and kustomizeconfig.yaml 

***Do not check the `secrets.yaml` file into git, keep it isolated in a secure
location or just delete it if the passwords contained are also kept elsewhere***


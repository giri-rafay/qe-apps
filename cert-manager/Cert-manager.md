**Cert-manager**

In order to access the application traffic securely through https, SSL certificates are necessary. These certificates are subjectible to expire over a specified period. The process of managing and generating/renewing the certificates is quite tedious. Cert-manager automates the complete process of managing SSL/TLS certificates for domains.

**Installation**

Cert-manager can be installed in both the cloud native methods such as applying the YAML manifest and also through helm package manager.

**Install using YAML Manifest**

Cert-manager is equipped with several resources including cert-manager, cainjector, webhook components and several custom reource definitions. All these components can be installed using the single mannifest file

```
kubectl apply -f https://github.com/jetstack/cert-manager/releases/download/v1.6.1/cert-manager.yaml
```

A new namespace `cert-manager` will be created and all the resources are accomodated in that namespace.

**Install using helm chart**

If you want to install using helm chart, follow the below steps.

1. Add the repository

```
helm repo add jetstack https://charts.jetstack.io
```

2. Update the repository

```
helm repo update
```

3. Download the helm chart.

```
helm fetch jetstack/cert-manager
```

4. cert-manager requires a number of CRD resources, which can be installed manually using `kubectl`, or using the `installCRDs` option when installing the Helm chart. Let us update the value in `values.yaml`

```
#Unzip the chart
tar -xzvf cert-manager-vx.x.x.tgz
#Navigate to the charts directory
cd cert-manager
```

5. Set the value of key `installCRDs` to `true` in `values.yaml` and save it.
6. Install the chart from the present directory after editing the `values.yaml` . By default, the resources are deployed in `default` namespace.

```
helm install cert-manager .
```

​        To deploy the resources in custom namespace for better isolation, use the command.

```
helm install \
  cert-manager . \
  --namespace cert-manager \
  --create-namespace 
```

​        This command will install the cert manager resources in separate namespace.

7. Check if it is installed successfully.

```
helm ls

NAME    	NAMESPACE	REVISION	UPDATED                             	STATUS  	CHART              	APP VERSION
cert-manager	default  	1       	2021-12-15 04:43:35.288758 +0530 IST	deployed	cert-manager-v1.6.1	v1.6.1     
bash-3.2$ 

```

The resources listed below are created.

```
kubectl -n cert-manager get all


NAME                                           READY   STATUS    RESTARTS   AGE
pod/cert-manager-57d89b9548-wnmrc              1/1     Running   0          42m
pod/cert-manager-cainjector-5bcf77b697-c9rmv   1/1     Running   0          42m
pod/cert-manager-webhook-9cb88bd6d-nt4h8       1/1     Running   0          42m

NAME                           TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)    AGE
service/cert-manager           ClusterIP   10.100.209.8   <none>        9402/TCP   42m
service/cert-manager-webhook   ClusterIP   10.100.36.74   <none>        443/TCP    42m

NAME                                      READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/cert-manager              1/1     1            1           42m
deployment.apps/cert-manager-cainjector   1/1     1            1           42m
deployment.apps/cert-manager-webhook      1/1     1            1           42m

NAME                                                 DESIRED   CURRENT   READY   AGE
replicaset.apps/cert-manager-57d89b9548              1         1         1       42m
replicaset.apps/cert-manager-cainjector-5bcf77b697   1         1         1       42m
replicaset.apps/cert-manager-webhook-9cb88bd6d       1         1         1       42m
bash-3.2$ 


```



**Configuration**

In general, 
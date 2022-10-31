#### ConfigMaps

Step-1

Create a ConfigMap named 'fresco-config'
Add key 'SERVER_URL'
Add value 'http://www.fresco.me'

verify if the ConfigMap is created

Step-2

create an nginx pod with environment variable 'SERVER_URL_ENV'
use the configMap created earlier, and assign the value to it. Use below template.

apiVersion: v1
kind: Pod
metadata: 
    name: fresco-nginx-pod
spec:
    containers:
        - name: fresco-nginx-container
          image: nginx
          env: fetch the value of server_URL_ENV from previous config map

Test your configuration by executing this command kubectl exec -it fresco-nginx-pod -- sh -c env | grep SERVER_URL_ENV

it should display:  http://www.fresco.me

### solution

- step-1: create a configmap ( imperative way )
    kubectl create configmap fresco-config --from-literal=SERVER_URL=http://www.fresco.me

  ( declarative way)

vi fresco-config.yaml

apiVersion: v1
kind: ConfigMap
metadata:
    name: fresco-config
data:
    SERVER_URL: "http://www.fresco.me"

CMD: kubectl create -f fresco-config.yaml

-now create pod

apiVersion: v1
kind: Pod
metadata:
    name: fresco-nginx-pod
spec:
    containers:
        - name: fresco-nginx-container
          image: nginx
          envFrom:
            - configMapRef:
                 name: fresco-config
                 key: SERVER_URL



#### Secrets

step-1

Create a secret 'fresco-secret' using:

data:
    user: admin
    pass: pass

step-2

Modify the above nginx pod to add the fresco-secret and mountPath /etc/test:
use this command to check if the pod and secret are successfully configured:

kubectl exec -it fresco-nginx-od -- sh -c "cat /etc/test/* | base64 -d"

it should display both username and password.

### Solution

( imperative way )
- kubectl create secret generic fresco-secret --from-literal=user=admin --from-literal=pass=cGFzcw

( Declarative way)

apiVersion: v1
kind: Secret
metadata:
    name: fresco-secret
data:
    test.file: |
        user: YWRtaW4=
        pass: cGFzcw==

- now add to pod

apiVersion: v1
kind: Pod
metadata:
    name: fresco-nginx-pod
spec:
    containers:
        - name: fresco-nginx-container
          image: nginx
          volumeMounts:
            - name: fresco-secret-volume
              mountPath: /etc
              readOnly: true
    volumes:
        - name: fresco-secret-volume
          secret:
            secretName: fresco-secret
            




#### Persistence Volume

Create a PV named 'fresco-pv' using the following parameters:

storageClassName - manual
capacity - 100MB
accessMode - ReadWriteOnce
hostPath - /temp/fresco

Create a PVC named 'fresco-pvc', and request for 50MB.
To verify successful creation, ensure it is bound to 'fresco-pv'

Modify above nginx pod named 'fresco-nginx-pod' using the following parameters:

Request for fresco-pvc as a volume
Use /usr/share/nginx/html for mount path. 

Hint: Use kubectl describe pod fresco-nginx-pod for debugging.

### Solution


#### RBAC

In this section you will create a user 'emp' and assign 'read' rights on pods belonging to the namespace 'dev'

Create a namespace named 'dev'
Use 'openssl' and create private key named 'emp.key'.

Create a certificate sign request named 'emp.csr' using the private key generated earlier.
Use the following information:

name: emp
group: dev

Generate 'emp.crt' by approving the request created earlier.

create a new context pointing to the cluster 'minikube' and name it 'dev-ctx'. it should point to the namespace 'dev'
and the user should be 'emp'.

set credentials for 'emp'.
Use 'emp.key' and 'emp.crt' created earlier.

Create a role named 'emp-role' and assign 'get' ,'list' access on 'pods' and  'deployments' (use 'dev' namespace).

Bind 'emp' to the role 'emp-role' created earlier, and name it 'emp-bind'.

Run an 'nginx' pod under the 'dev-ctx' and 'dev' namespace and 'nginx' name.

Execute: kubectl --context==dev-ctx get pods -o wide and ensure it is deployed.

if you try to execute 'kubectl --context=dev.ctx get pods -n default' , a forbidden error appears. This is because
only employees are authorized to access the 'dev' namespace.

### Solution


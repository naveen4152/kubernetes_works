#### Imperative commands

#### POD : create an nginx pod

- kubectl run nginx --image=nginx

#### generate POD Manifest YAML file (-o yaml). Don't create it(--dry-run)

- kubectl run nginx --image=nginx --dry-run=client -o yaml > pod-def.yaml

#### Deployment : create a deployment

- kubectl create deployment --image=nginx

#### generate Deployment YAML file (-o yaml). Dont create it (--dry-run)

- kubectl create deployment --image=nginx --dry-run -o yaml > deployment-def.yaml

#### generate Deployment with 4 replicas

- kubectl create deployment --image=nginx --replicas=4 --dry-run=client -o yaml > dep.yaml

Another way to do this is to create yaml file and modify replicas to 4 and apply or use kubectl scale.

#### service : create a service named redis-sevice of type clusterip to expose pod redis on port=6379

- kubectl expose pod redis --port=6379 --name redis-sevice --dry-run=client -o yaml

or

- kubectl create service clusterip redis --tcp=6379:6379 --dry-run=client -o yaml
this will asumme redis as selector. this will not work if pod has differcnt label so create file and modify the selector.

#### create a service named nginx of type nodeport to expose pod nginx port 80 on port 30080 on the nodes:

- kubectl expose pod nginx --port=80 --name nginx-service --type=NodePort --dry-run=client -o yaml

or

- kubectl create service nodeport nginx --tcp==80:80 --node-port=3000 --dry-run=client -o yaml

Both the above commands have their own challenges. While one of it cannot accept a selector the other cannot accet a
nodeport. recommended approch is going with kubectl expose  command. if u need to specify a node ort, generate a def file
using the same command and manually inut the nodeport before creating the service.


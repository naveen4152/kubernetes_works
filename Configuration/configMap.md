#### create config map with command ( imperative way )

- kubectl create configmap app-config --from-literal=APP_COLOR=blue --from-literal=APP_MOD=prod

#### Declarative approach

- create config-map.yaml

apiVersion: v1
kind: ConfigMap
metadata:
    name: app-config
data:
    APP_COLOR=blue
    APP_MOD=prod

- kubectl create -f config-map.yaml

- create pod-def.yaml

apiVersion: v1
kind: pod
metadata:
    name: simple-webapp-color
    labels:
        name: simple-webapp-color
spec:
    containers:
        - name: simple-webapp-color
          image: simple-webapp-color
          ports: 
            - containerPort: 8080
          envFrom:
            - configMapRef:
                name: app-config



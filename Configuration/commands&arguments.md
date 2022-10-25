#### commands and arguments in Kubernetes pod

Dockerfile:

FROM ubuntu
ENTRYPOINT ["sleep"]
CMD ["5"]

pod-definition.yaml

apiVersion: v1
kind: Pod
metadata:
    name: ubuntu-sleeper
spec:
    - name: ubuntu-sleeper
      image: ubuntu
      command: ["this will override entrypoint in dockerfile"]
      args: ["10"]

- The arguments and command given in pod file will override cmd and entrypoint in dockerfile.

#### Environment variables

- docker run -e APP_COLOR=pink sample-webapp-color
    
        in above docker run command shows how to set env in docker 

pod-definition.yaml

apiVersion: v1
kind: Pod
metadata:
    name: sample-webapp-color
spec:
    - name: simple-webapp-color
      image: simple-webapp-color
      ports:
        - containerPort: 8080
      env:
        - name: APP_COLOR
          value: pink

- Environment variable using configmaps

pod-definition.yaml

apiVersion: v1
kind: Pod
metadata:
    name: sample-webapp-color
spec:
    - name: simple-webapp-color
      image: simple-webapp-color
      ports:
        - containerPort: 8080
      
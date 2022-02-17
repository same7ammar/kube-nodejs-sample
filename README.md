# About

Simple Nodejs app to deploy it to Kubernetes cluster or Docker container.
## without Installtion.
```sh
$ kubectl apply -f https://raw.githubusercontent.com/same7ammar/nodejs-sample-k8s/master/kubernetes/
$ kubectl get all
NAME                                  READY   STATUS    RESTARTS   AGE
pod/nodejs-web-app-6fdf6d4f54-m4mgn   1/1     Running   0          20m

NAME                     TYPE           CLUSTER-IP      EXTERNAL-IP      PORT(S)          AGE
service/nodejs-web-app   LoadBalancer   10.43.240.254   172.17.152.146   3000:30043/TCP   6m58s

NAME                                  DESIRED   CURRENT   READY   UP-TO-DATE   AVAILABLE   NODE SELECTOR   AGE
daemonset.apps/svclb-nodejs-web-app   1         1         1       1            1           <none>          6m58s

NAME                             READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/nodejs-web-app   1/1     1            1           20m

NAME                                        DESIRED   CURRENT   READY   AGE
replicaset.apps/nodejs-web-app-6fdf6d4f54   1         1         1       20m
$ curl http://EXTERNAL-IP:PORT
$ curl http://172.17.152.146  
Hello World
```
## Installation localy using Rancher Destkop

clone to your local machine.

```bash
git clone https://github.com/same7ammar/nodejs-sample-k8s.git
```

## Build  your docker image :

```sh
$ cd app
$ docker build . -t <your username>/nodejs-sample-k8s
$ docker run -p 3000:3000-d <your username>/nodejs-sample-k8s
# Get container ID
$ docker ps
$ curl curl http://localhost:3000
Hello World
```
## Upload image to   your docker-Hub Account :
```sh
$ docker login -u same7ammar  --password-stdin 
Login Succeeded
$ docker push <your username>/nodejs-sample-k8s
Using default tag: latest
The push refers to repository [docker.io/same7ammar/node-web-app-k8s]
.....
latest: digest: sha256:fa5dd972a9cd1555f3cb4a837aaf5d78bc862fa0053474d9f64f3e7d3eb15ae2 size: 3048

```
## Deploy your app to kuberneters cluster - I'm using Rancher Desktop. 
1. create a deployment .
```sh 
$ kubectl apply -f kubernetes/deployment.yaml 
deployment.apps/nodejs-web-app created

$  kubectl get pods
NAME                              READY   STATUS              RESTARTS   AGE
nodejs-web-app-6fdf6d4f54-m4mgn   0/1     ContainerCreating   0          10s

$ kubectl get pods
NAME                              READY   STATUS    RESTARTS   AGE
nodejs-web-app-6fdf6d4f54-m4mgn   1/1     Running   0          2m18s

```
2. create a service to expose this app .
```sh
$ kubectl apply -f kubernetes/service.yaml
service/nodejs-web-app created

$ kubectl get svc
NAME             TYPE           CLUSTER-IP      EXTERNAL-IP      PORT(S)          AGE
nodejs-web-app   LoadBalancer   10.43.240.254   172.17.152.146   3000:30043/TCP   7s
to test service use  curl http://EXTERNAL-IP:PORT
$ curl http://172.17.152.146 
Hello World
```

## Contributing
Pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change.

Please make sure to update tests as appropriate.

## License
[MIT](https://choosealicense.com/licenses/mit/)
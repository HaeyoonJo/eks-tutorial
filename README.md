# EKS Tutorial

Simply I could provision EKS and service using `eksctl`. At this stage, I will create an EKS cluster and explore the provisioned resources.


All yaml files were cloned from eks-nginx [repo](https://github.com/cohenaj194/eks-nginx.git)

### 1. EKS cluster and deploy Nginx service

- Create EKS Cluster
```
eksctl create cluster -f cluster.yaml
```

```
kubectl get svc
NAME         TYPE        CLUSTER-IP   EXTERNAL-IP   PORT(S)   AGE
kubernetes   ClusterIP   10.100.0.1   <none>        443/TCP   25m
```

- Deploy Nginx service

```
kubectl apply -f ./run-my-nginx.yaml
```

- Check created pods, nodes, deployment, service
```
$ kubectl get deployment
NAME       READY   UP-TO-DATE   AVAILABLE   AGE
my-nginx   2/4     4            2           4m54s

 kubectl get nodes
NAME                                              STATUS   ROLES    AGE   VERSION
ip-192-168-0-159.eu-central-1.compute.internal    Ready    <none>   13m   v1.22.9-eks-810597c
ip-192-168-38-100.eu-central-1.compute.internal   Ready    <none>   13m   v1.22.9-eks-810597c

kubectl get pods
NAME                        READY   STATUS    RESTARTS   AGE
my-nginx-5b56ccd65f-gwkwn   0/1     Pending   0          5m14s
my-nginx-5b56ccd65f-k62jx   1/1     Running   0          5m14s
my-nginx-5b56ccd65f-x5pjx   1/1     Running   0          5m14s
my-nginx-5b56ccd65f-xgf4w   0/1     Pending   0          5m14s

kubectl get pods -o wide
NAME                        READY   STATUS    RESTARTS   AGE     IP               NODE                                             NOMINATED NODE   READINESS GATES
my-nginx-5b56ccd65f-gwkwn   0/1     Pending   0          5m23s   <none>           <none>                                           <none>           <none>
my-nginx-5b56ccd65f-k62jx   1/1     Running   0          5m23s   192.168.23.182   ip-192-168-0-159.eu-central-1.compute.internal   <none>           <none>
my-nginx-5b56ccd65f-x5pjx   1/1     Running   0          5m23s   192.168.13.206   ip-192-168-0-159.eu-central-1.compute.internal   <none>           <none>
my-nginx-5b56ccd65f-xgf4w   0/1     Pending   0          5m23s   <none>           <none>                                           <none>           <none>
```

### 2. Expose Nginx service
Once it's connected to LB, then you will be able to access its Nginx service.

```
kubectl expose deployment/my-nginx \
        --port=80 --target-port=80 \
        --name=my-nginx-service --type=LoadBalancer
```

Check if Nginx service was created
```
kubectl get services
NAME               TYPE           CLUSTER-IP       EXTERNAL-IP                                                                 PORT(S)        AGE
kubernetes         ClusterIP      10.100.0.1       <none>                                                                      443/TCP        28m
my-nginx-service   LoadBalancer   10.100.223.178   a305745700b094883bfe2281b974429a-411902197.eu-central-1.elb.amazonaws.com   80:30503/TCP   13s
```
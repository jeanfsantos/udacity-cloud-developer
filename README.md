## Simple Express & Nginx Reverse Proxy:

- access Circle CI platform to build simple-reverse-proxy and simple-express images:

- go to directory simple-reverse-proxy 
```
kubectl apply -f deployment.yml
kubectl apply -f service.yml
```

- go to directory simple-express
```
kubectl apply -f deployment.yml
kubectl apply -f service.yml
```

### check

kubectl get pods
```
NAME                            READY   STATUS    RESTARTS   AGE
my-app-2-64f56bf5bb-9q77r       1/1     Running   0          27m
reverseproxy-7674f5b948-7l4gl   1/1     Running   0          27m
```

kubectl get services
```
NAME               TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)    AGE
kubernetes         ClusterIP   10.100.0.1       <none>        443/TCP    173m
my-app-2-svc       ClusterIP   10.100.96.132    <none>        8080/TCP   89m
reverseproxy-svc   ClusterIP   10.100.188.174   <none>        8080/TCP   37m
```

kubectl exec -it my-app-2-64f56bf5bb-9q77r bash
```
root@my-app-2-64f56bf5bb-9q77r:/usr/src/app# curl reverseproxy-svc:8080/api/health
Hello!
```

kubectl describe services
```
Name:              kubernetes
Namespace:         default
Labels:            component=apiserver
                   provider=kubernetes
Annotations:       <none>
Selector:          <none>
Type:              ClusterIP
IP Family Policy:  SingleStack
IP Families:       IPv4
IP:                10.100.0.1
IPs:               10.100.0.1
Port:              https  443/TCP
TargetPort:        443/TCP
Endpoints:         172.31.5.186:443,172.31.80.76:443
Session Affinity:  None
Events:            <none>


Name:              my-app-2-svc
Namespace:         default
Labels:            app=my-app-2
Annotations:       <none>
Selector:          app=my-app-2
Type:              ClusterIP
IP Family Policy:  SingleStack
IP Families:       IPv4
IP:                10.100.96.132
IPs:               10.100.96.132
Port:              <unset>  8080/TCP
TargetPort:        8080/TCP
Endpoints:         172.31.73.96:8080
Session Affinity:  None
Events:            <none>


Name:              reverseproxy-svc
Namespace:         default
Labels:            app=reverseproxy
Annotations:       <none>
Selector:          app=reverseproxy
Type:              ClusterIP
IP Family Policy:  SingleStack
IP Families:       IPv4
IP:                10.100.188.174
IPs:               10.100.188.174
Port:              8080  8080/TCP
TargetPort:        8080/TCP
Endpoints:         172.31.71.119:8080
Session Affinity:  None
Events:            <none>
```


### clean up
```
kubectl delete service my-app-2-svc  
kubectl delete service reverseproxy-svc  
kubectl delete deployment my-app-2  
kubectl delete deployment reverseproxy  
```

### Troubleshooting:

to check details about the IAM user or role whose credentials are used to call the operation.

```
$ aws sts get-caller-identity 
```

can use optional param to set profile, e.g. --profile udacity

to creating or updating a kubeconfig file for an Amazon EKS cluster

```
aws eks update-kubeconfig --region us-east-1 --name MyEKSCluster
```

for kubenetes commands: https://kubernetes.io/docs/reference/kubectl/cheatsheet/
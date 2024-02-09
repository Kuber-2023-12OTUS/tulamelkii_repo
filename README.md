# Выполнено ДЗ №

 - [X] Основное ДЗ
 - [ ] Задание со *

## В процессе сделано:
```
- create readiness-probe httpGet
- create service.yaml for ClusterIP
- create ingress-controller
- create ingress.yaml for homework.otus
```
## Как запустить проект:
- create raidness-probe
```
- kubectl apply -f deployment.yaml -n homework
  readinessProbe:
    httpGet:
      path: /index.html
      port: 8000
  Readiness:    http-get http://:8000/index.html delay=3s timeout=1s period=4s #success=1 #failure=3
```
- create service.yaml for ClusterIP
```
- kubectl apply -f service.yaml 
- kubectl describe svc

 Name:              server-nginx
 Namespace:         default
 Labels:            <none>
 Annotations:       <none>
 Selector:          app=nginx
 Type:              ClusterIP
 IP Family Policy:  SingleStack
 IP Families:       IPv4
 IP:                10.106.237.179
 IPs:               10.106.237.179
 Port:              web  80/TCP
 TargetPort:        8000/TCP
 Endpoints:         <none>
 Session Affinity:  None
 Events:            <none>

- kubectl get svc
  NAME           TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)   AGE
  kubernetes     ClusterIP   10.96.0.1        <none>        443/TCP   12d
  server-nginx   ClusterIP   10.106.237.179   <none>        80/TCP    2s

```
- create ingress-controller
```
- minikube addons enable ingress
```
- create ingress.yaml for homework.otus
```
- kubectl apply -f ingress.yaml 

- kubectl get ingress
  NAME           CLASS   HOSTS           ADDRESS        PORTS   AGE
  ingress-host   nginx   homework.otus   192.168.49.2   80      4h26m

- kubectl describe ingress 


  
```
## Как проверить работоспособность:
-


## PR checklist:
 - [ ] Выставлен label с темой домашнего задания

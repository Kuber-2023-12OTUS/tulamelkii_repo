# Выполнено ДЗ №

 - [X] Основное ДЗ
 - [ ] Задание со *

## В процессе сделано:
```
- create readiness-probe httpGet
- create service.yaml for ClusterIP
- create ingress-controller
- create minikube tunnel
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
- Create minikube tunnel
```
- minikube tunnel

💡  Exiting due to TUNNEL_ALREADY_RUNNING: Another tunnel process is already running, terminate the existing instance to start a new one

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
- curl service ip
  curl 10.104.205.177
```
<!DOCTYPE HTML>
<html lang="en">
<head>
<!-- THIS IS A COMMENT -->
<title>Sample Web Page</title>
<META charset="utf-8">
<META name="viewport"
 content="width=device-width, initial-scale=1.0">
<style>
blockquote { margin-left:20px; margin-right:5px }
pre { overflow-x:auto }
.tt { font-family:monospace }
.nowrap { white-space:nowrap }
.example { font-family:monospace; white-space:pre; overflow-x:auto; }
h3 { border-top:1px solid grey }
blockquote { margin-top:0; margin-bottom:0 }
table.compact { border-collapse:collapse }
table.compact th { text-align:left; background:#eeeeee }
table.compact td,th { padding:0 4px 0 8px; border:1px solid grey }
</style>
</head>
....

```
  


## PR checklist:
 - [ ] Выставлен label с темой домашнего задания

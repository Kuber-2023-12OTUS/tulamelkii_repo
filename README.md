# Выполнено ДЗ № 3

 - [X] Основное ДЗ
 - [X] Задание со *

## В процессе сделано:
```
- create readiness-probe httpGet
- create service.yaml for ClusterIP
- create ingress-controller
- create minikube tunnel
- create ingress.yaml for homework.otus
- create redirect homework.otus
```
## Как запустить проект:
- create raidness-probe
```
- kubectl apply -f deployment.yaml 
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
 IP:                10.96.125.147
 IPs:               10.96.125.147
 Port:              web  80/TCP
 TargetPort:        8000/TCP
 Endpoints:         10.244.0.212:8000,10.244.0.213:8000,10.244.0.214:8000
 Session Affinity:  None
 Events:            <none>


- kubectl get svc -o=wide

 NAME           TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)   AGE   SELECTOR
 kubernetes     ClusterIP   10.96.0.1       <none>        443/TCP   12d   <none>
 server-nginx   ClusterIP   10.96.125.147   <none>        80/TCP    98m   app=nginx

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
  ingress-host   nginx   homework.otus   192.168.49.2   80      6h54m

- kubectl describe ingress
  ame:             ingress-host
  Labels:           <none>
  Namespace:        default
  Address:          192.168.49.2
  Ingress Class:    nginx
  Default backend:  <default>
  Rules:
  Host           Path  Backends
  ----           ----  --------
  homework.otus  
                 /                server-nginx:80 (10.244.0.212:8000,10.244.0.213:8000,10.244.0.214:8000)
                 /homework/(.*)   server-nginx:80 (10.244.0.212:8000,10.244.0.213:8000,10.244.0.214:8000)
  Annotations:     nginx.ingress.kubernetes.io/rewrite-target: /$1
                 nginx.ingress.kubernetes.io/use-regex: true
  Events:
  Type    Reason  Age                  From                      Message
  ----    ------  ----                 ----                      -------
  Normal  Sync    17m (x22 over 140m)  nginx-ingress-controller  Scheduled for sync
  
```
## Как проверить работоспособность:
- curl service ip
- curl 10.96.125.147
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
- minikube ssh
- curl http://homework.otus
- curl -I http://homework.otus/index.html
```
HTTP/1.1 200 OK
Date: Fri, 09 Feb 2024 08:07:49 GMT
Content-Type: text/html
Content-Length: 34974
Connection: keep-alive
Last-Modified: Fri, 09 Feb 2024 06:21:01 GMT
ETag: "65c5c44d-889e"
Accept-Ranges: bytes

<!DOCTYPE HTML>
<html lang="en">
<head>
<!-- THIS IS A COMMENT -->
<title>Sample Web Page</title>
<META charset="utf-8">
<META name="viewport"
 content="width=device-width, initial-scale=1.0">
<style>
```
## PR checklist:
 - [ ] Выставлен label с темой домашнего задания

# Выполнено ДЗ №

 - [X] Основное ДЗ
 - [X] Задание со *

## В процессе сделано:
```
 - Created vm in yandex cloud
 - Created cluster minikube version: v1.32.0
 - Installed kubectl
 - Created docker image nginx : tulamelki/neweng (with port 8000 and edit config nginx)
 - Created Namespace: homework (manifest namespace.yaml)
 - Created Pod (static-web)
 - Created init container (image: busybox)
   - Share volume without two containers( for web-nginx /homework and for init /init)
   - Init container download page index.html and put in share volume
 - Before close container web-nginx preStop removed index.html
```
## Как запустить проект:
```
 - minikube start
 - kubectl cluster-info
   * Kubernetes control plane is running at https://192.168.148.2:8443
   * CoreDNS is running at https://192.168.148.2:8443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
 - kubectl create -f namespace.yaml
 - kubectl get namespace
    NAME              STATUS   AGE
    default           Active   8d
  * homework          Active   5d19h
    kube-node-lease   Active   8d
    kube-public       Active   8d
    kube-system       Active   8d
- kubectl apply -f pod.yaml --namespace=homework
   kubectl get pods --namespace=homework
   NAME         READY   STATUS    RESTARTS   AGE
   static-web   1/1     Running   0          38m
- kubectl describe pod static-web --namespace=homework`
```

```
Name:             static-web
Namespace:        homework
Priority:         0
Service Account:  default
Node:             minikube/192.168.148.2
Start Time:       Sun, 21 Jan 2024 14:18:33 +0000
Labels:           <none>
Annotations:      <none>
Status:           Running
IP:               10.244.0.23

- kubectl exec -it static-web --namespace=homework  bash
```
## Как проверить работоспособность:
 ```
 - netstat -tuln
         
   Active Internet connections (only servers)
   Proto Recv-Q Send-Q Local Address           Foreign Address         State
   tcp        0      0 0.0.0.0:8000            0.0.0.0:*               LISTEN
   tcp6       0      0 :::8000                 :::*                    LISTEN     

 - curl http://localhost:8000
 - Check init download index.html and save share volume
 - Check page: 
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
.....
```
 ## PR checklist:
 - [ ] Выставлен label с темой домашнего задания

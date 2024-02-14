# Выполнено ДЗ № 3

 - [X] Основное ДЗ
 - [X] Задание со *

## В процессе сделано:
```
- create manifest pvc.yaml for PersistentVolumeClaim
- create manifest cm.yaml for
- edit  deployment.yaml (umount emptyDir and mount pvc)
- edit deployment and add configMap with volume for directory /homework/conf
- create storage class with: k8s.io/minikube-hostpath and edit Policy:reclaimPolicy Retain
- edit  manifest pvc for my storageClass
```
## Как запустить проект:
- create manifest pvc.yaml for PersistentVolumeClaim
- storage 4G storage class "myprovision"
```
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: pvc-volume2
spec:
  storageClassName: "myprovision"
  accessModes:
  - ReadWriteOnce
  resources:
    requests:
      storage: 4Gi
```
```
- kubectl get pvc -o=wide
  pvc-volume2   Bound  pvc-5ccedf45-11a2-428f-8c29-d79bb33f2633  4Gi  RWO  myprovision  45h  Filesystem

- kubectl get pv
  NAME                                       CAPACITY   ACCESS MODES   RECLAIM POLICY   STATUS   CLAIM                 STORAGECLASS   REASON   AGE
 pvc-5ccedf45-11a2-428f-8c29-d79bb33f2633   4Gi        RWO            Retain           Bound    default/pvc-volume2    myprovision             45h
 pvc-73a56c8d-51dd-4972-a937-0ec81fcbd256   2Gi        RWO            Delete           Bound    default/pvc-volume     standard                2d

- kubectl describe pvc
  Name:          pvc-volume2
  Namespace:     default
  StorageClass:  myprovision
  Status:        Bound
  Volume:        pvc-5ccedf45-11a2-428f-8c29-d79bb33f2633
  Labels:        <none>
  Annotations:   pv.kubernetes.io/bind-completed: yes
                 pv.kubernetes.io/bound-by-controller: yes
                 volume.beta.kubernetes.io/storage-provisioner: k8s.io/minikube-hostpath
                 volume.kubernetes.io/storage-provisioner: k8s.io/minikube-hostpath
  Finalizers:    [kubernetes.io/pvc-protection]
  Capacity:      4Gi
  Access Modes:  RWO
  VolumeMode:    Filesystem
  Used By:       web-deploy-975b86874-529vj
                 web-deploy-975b86874-6wjpb
                 web-deploy-975b86874-qbzt4
  Events:        <none>

```
- create manifest storageClass.yaml with provisioner: k8s.io/minikube-hostpath
- with polycy: Retain
```
apiVersion: storage.k8s.io/v1
kind: StorageClass
metadata:
  name: myprovision
  labels:
    name: mystor
provisioner: k8s.io/minikube-hostpath
reclaimPolicy: Retain
volumeBindingMode: Immediate
```
```
- kubectl describe sc
  Name:            myprovision
  IsDefaultClass:  No
  Annotations:     kubectl.kubernetes.io/last-applied-configuration={"apiVersion":"storage.k8s.io /v1","kind":"StorageClass","metadata":{"annotations":{},"labels": {"name":"mystor"},"name":"myprovision"},"provisioner":"k8s.io/minikube- hostpath","reclaimPolicy":"Retain","volumeBindingMode":"Immediate"}
,storageclass.kubernetes.io/is-default-class=false
  Provisioner:           k8s.io/minikube-hostpath
  Parameters:            <none>
  AllowVolumeExpansion:  <unset>
  MountOptions:          <none>
  ReclaimPolicy:         Retain
  VolumeBindingMode:     Immediate
  Events:                <none>
```
- create cm.yaml and mount in directory /homework/conf/file
- this is config  new html page in configmap
```
- mountPath: /homework/conf/file
  name: env
  subPath: file

apiVersion: v1
data:
  file: |+
    <!doctype html>
    <html lang="en-US">
      <head>
        <meta charset="utf-8" />
        <meta name="viewport" content="width=device-width" />
        <title>My World</title>
      </head>
      <body>
        <img src="images/firefox-icon.png" alt="My test image" />
      </body>
    </html>

kind: ConfigMap
metadata:
  creationTimestamp: null
  name: cm

- kubectl get cm
  NAME               DATA   AGE
  cm                 1      5m27s
  cmnginx            1      4m54s
  kube-root-ca.crt   1      18d
```
- create new config nginx (cmnginx.yaml)
- this is config map mount file with new preference nginx (home.conf in pods)
```
- mountPath: /etc/nginx/conf.d/home.conf
  name: nginx
  subPath: home.conf
  readOnly: true


- kubectl describe cm cmnginx
  Name:         cmnginx
  Namespace:    default
  Labels:       <none>
  Annotations:  <none>

  Data
  ====
  home.conf:
  ----
  server {
    listen       8000;
    listen  [::]:8000;
    server_name  localhost;

    location / {
        root   /homework;
        index  index.html index.htm;
    }
    location /conf/file {
        root  /homework;
   }
  }

  BinaryData
  ====
  Events:  <none>

```
- edit deploy and add PersistentVolumeClaim for container init and default container 

```
  containers:
      - name: webnginx          
        image: tulamelki/neweng
        lifecycle:
          preStop:                  
            exec:
              command: [ 'sh', '-c', 'rm /homework/index.html'] 
        ports:
        - containerPort: 8000
        volumeMounts:
        - mountPath: /homework
          name: volume
....
 initContainers:         
      - name: init-containers      
        image: busybox             
        command: ['sh', '-c', 'wget http://www.columbia.edu/~fdc/sample.html -O /init/index.html']
        volumeMounts:
        - mountPath: /init         
          name: volume
      volumes:
      - name: volume 
        persistentVolumeClaim:
           claimName: pvc-volume2 
```

## Как проверить работоспособность:
- check pod
- kubectl get po (pods runing)
```
NAME                          READY   STATUS    RESTARTS   AGE
web-deploy-568894b964-2xf2r   1/1     Running   0          34m
web-deploy-568894b964-k85nt   1/1     Running   0          34m
web-deploy-568894b964-l2dtl   1/1     Running   0          34m
```
- check service
- kubectl get svc (service work)
```
NAME           TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)   AGE
kubernetes     ClusterIP   10.96.0.1        <none>        443/TCP   18d
server-nginx   ClusterIP   10.100.173.218   <none>        80/TCP    3h31m

```
DONT forget enable minikube tunnel!
```
- curl 10.100.173.218
<!DOCTYPE HTML>
<html lang="en">
<head>
<!-- THIS IS A COMMENT -->
<title>Sample Web Page</title>
<META charset="utf-8">
<META name="viewport"
```
- check ingress
- 
```

```
## PR checklist:
 - [ ] Выставлен label с темой домашнего задания

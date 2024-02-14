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
- create cm.yaml
- this is config  new html page in configmap
```
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
- this is config change preference nginx in home.conf for pods
```
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

```

```
## Как проверить работоспособность:

```
## PR checklist:
 - [ ] Выставлен label с темой домашнего задания

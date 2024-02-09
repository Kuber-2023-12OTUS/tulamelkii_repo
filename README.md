Задание N2 kubernetes-controllers

Выполнено ДЗ №

   [X] Основное ДЗ
   
   [x]Задание со *

В процессе сделано:
```
    Create manifest namespace.yaml namespace=homework
    Create deployment.yaml
    Create deploy 3 pods (nginx)
    Create readiness, it check file /homework/index.html (every 4 seconds cat file)
    Create RollingUpdate,During the update process, a maximum of 1 pod
    Create labels homework=true for node minikube and add nodeSelector - homework: "true"
```
Как запустить проект:
```
  - kubectl create -f namespace.yaml
    vagrant@minikube:~$ kubectl get namespace
    NAME STATUS AGE
    default Active 27h
    homework Active 4m58s
    kube-node-lease Active 27h
    kube-public Active 27h
    kube-system Active 27h

   - kubectl apply -f deployment.yaml --namespace=homework
      vagrant@minikube:~$ kubectl get deploy --namespace=homework

    NAME READY UP-TO-DATE AVAILABLE AGE
    web-deploy 3/3 3 3 2m2s
    vagrant@minikube:~$ kubectl describe deploy --namespace=homework
    Name: web-deploy
    Namespace: homework
    CreationTimestamp: Sun, 28 Jan 2024 21:11:11 +0000
    Labels:
    Annotations: deployment.kubernetes.io/revision: 1
    Selector: app=nginx
    Replicas: 3 desired | 3 updated | 3 total | 3 available | 0 unavailable
    StrategyType: RollingUpdate
    MinReadySeconds: 0
    RollingUpdateStrategy: 1 max unavailable, 25% max surge

    - kubectl label nodes minikube homework=true
    - kubectl get nodes --show-labels
    vagrant@minikube:~$ kubectl get nodes --show-labels NAME STATUS ROLES AGE VERSION LABELS minikube Ready control-plane 29h v1.28.3 beta.kubernetes.io/arch=amd64,beta.kubernetes.io/ os=linux,homework=true,kubernetes.io/arch=amd64,kubernetes.io /hostname=minikube,kubernetes.io/os=linux,mini
```
Как проверить работоспособность:
```
    if i delete index.html , then status not ready

    - kubectl get pods --namespace=homework

    NAME READY STATUS RESTARTS AGE
    web-deploy-5dc 0/1 Running 0 26m
    web-deploy-5dc 1/1 Running 0 26m
    web-deploy-5dcf 1/1 Running 0 26m

    i have 1 node minikube and i don't check create or not pod with lables homework=true
```
PR checklist:

    Выставлен label с темой домашнего задания


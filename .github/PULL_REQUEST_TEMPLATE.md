# Выполнено ДЗ №

 - [X] Основное ДЗ
 - [ ] Задание со *

## В процессе сделано:
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
## Как запустить проект:
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
- kubectl describe pod static-web --namespace=homework
******************************************************info on pod*******************************************  
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
IPs:
  IP:  10.244.0.23
Init Containers:
  init-containers:
    Container ID:  docker://c22637114f518077cfc16fc8c228d01e4d1068897975d8320fd294a5cf33fd20
    Image:         busybox
    Image ID:      docker-pullable://busybox@sha256:6d9ac9237a84afe1516540f40a0fafdc86859b2141954b4d643af7066d598b74
    Port:          <none>
    Host Port:     <none>
    Command:
      sh
      -c
      wget http://www.columbia.edu/~fdc/sample.html -O /init/index.html
    State:          Terminated
      Reason:       Completed
      Exit Code:    0
      Started:      Sun, 21 Jan 2024 14:18:35 +0000
      Finished:     Sun, 21 Jan 2024 14:18:36 +0000
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /init from volume (rw)
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-grf2q (ro)
Containers:
  web-nginx:
    Container ID:   docker://45b8284992e66f9de390e685ffc103296d77fdcf532c6345aeb669131bd06843
    Image:          tulamelki/neweng
    Image ID:       docker-pullable://tulamelki/neweng@sha256:aa6babc55e0ff74793b821cdef9039b40dfb77adc0767f1daac2ea1aaf85bf47
    Port:           8000/TCP
    Host Port:      0/TCP
    State:          Running
      Started:      Sun, 21 Jan 2024 14:18:37 +0000
    Ready:          True
    Restart Count:  0
    Environment:    <none>
    Mounts:
      /homework from volume (rw)
      /var/run/secrets/kubernetes.io/serviceaccount from kube-api-access-grf2q (ro)
Conditions:
  Type              Status
  Initialized       True 
  Ready             True 
  ContainersReady   True 
  PodScheduled      True 
Volumes:
  volume:
    Type:       EmptyDir (a temporary directory that shares a pod's lifetime)
    Medium:     
    SizeLimit:  800Mi
  kube-api-access-grf2q:
    Type:                    Projected (a volume that contains injected data from multiple sources)
    TokenExpirationSeconds:  3607
    ConfigMapName:           kube-root-ca.crt
    ConfigMapOptional:       <nil>
    DownwardAPI:             true
QoS Class:                   BestEffort
Node-Selectors:              <none>
Tolerations:                 node.kubernetes.io/not-ready:NoExecute op=Exists for 300s
                             node.kubernetes.io/unreachable:NoExecute op=Exists for 300s
Events:
  Type    Reason     Age   From               Message
  ----    ------     ----  ----               -------
  Normal  Scheduled  45m   default-scheduler  Successfully assigned homework/static-web to minikube
  Normal  Pulling    45m   kubelet            Pulling image "busybox"
  Normal  Pulled     45m   kubelet            Successfully pulled image "busybox" in 1.258s (1.258s including waiting)
  Normal  Created    45m   kubelet            Created container init-containers
  Normal  Started    45m   kubelet            Started container init-containers
  Normal  Pulling    45m   kubelet            Pulling image "tulamelki/neweng"
  Normal  Pulled     45m   kubelet            Successfully pulled image "tulamelki/neweng" in 1.263s (1.263s including waiting)
  Normal  Created    45m   kubelet            Created container web-nginx
  Normal  Started    45m   kubelet            Started container web-nginx
*************************************************************************************************************

 
## Как проверить работоспособность:
 - curl http://localhost:8000
   

## PR checklist:
 - [ ] Выставлен label с темой домашнего задания

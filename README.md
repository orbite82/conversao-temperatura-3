# conversao-temperatura-3
 Desafio 01 - Docker

# Create Cluster K3D

´´´
:~/Iniciativa-Devops/temperatura/conversao-temperatura-3
└──> $ k3d cluster create meucluster --servers 3 --agents 3
´´´

´´´
~/Iniciativa-Devops/temperatura/conversao-temperatura-3
└──> $ docker container ls
CONTAINER ID   IMAGE                            COMMAND                  CREATED              STATUS              PORTS                             NAMES
7ff472b4deaa   ghcr.io/k3d-io/k3d-proxy:5.4.4   "/bin/sh -c nginx-pr…"   About a minute ago   Up 45 seconds       80/tcp, 0.0.0.0:34431->6443/tcp   k3d-meucluster-serverlb
87d37b3bfc0c   rancher/k3s:v1.23.8-k3s1         "/bin/k3s agent"         About a minute ago   Up 53 seconds                                         k3d-meucluster-agent-2
5e28f3f794d3   rancher/k3s:v1.23.8-k3s1         "/bin/k3s agent"         About a minute ago   Up 53 seconds                                         k3d-meucluster-agent-1
6b49641fef57   rancher/k3s:v1.23.8-k3s1         "/bin/k3s agent"         About a minute ago   Up 53 seconds                                         k3d-meucluster-agent-0
9c3b727f77b8   rancher/k3s:v1.23.8-k3s1         "/bin/k3s server --t…"   About a minute ago   Up About a minute                                     k3d-meucluster-server-2
534bec94262f   rancher/k3s:v1.23.8-k3s1         "/bin/k3s server --t…"   About a minute ago   Up About a minute                                     k3d-meucluster-server-1
6220091babf7   rancher/k3s:v1.23.8-k3s1         "/bin/k3s server --c…"   About a minute ago   Up About a minute                                     k3d-meucluster-server-0


~/Iniciativa-Devops/temperatura/conversao-temperatura-3
└──> $ k3d cluster list
NAME         SERVERS   AGENTS   LOADBALANCER
meucluster   3/3       3/3      true
´´´

# Apply no pod.yaml

´´´
┌~/Iniciativa-Devops/temperatura/conversao-temperatura-3/k8s
└──> $ kubectl apply -f pod.yaml
pod/meupod created
pod/meupod-2 created

~/Iniciativa-Devops/temperatura/conversao-temperatura-3/k8s
└──> $ kubectl get pods
NAME       READY   STATUS              RESTARTS   AGE
meupod     0/1     ContainerCreating   0          13s
meupod-2   0/1     ContainerCreating   0          13s

´´´

# APP Green

´´´
:~/Iniciativa-Devops/temperatura/conversao-temperatura-3/k8s
└──> $ kubectl get pods -l app=green
NAME       READY   STATUS    RESTARTS   AGE
meupod-2   1/1     Running   0          6m24s

~/Iniciativa-Devops/temperatura/conversao-temperatura-3/k8s
└──> $ kubectl port-forward pod/meupod-2 8080:80
Forwarding from 127.0.0.1:8080 -> 80
Forwarding from [::1]:8080 -> 80
´´´
# Teste

http://localhost:8080/


# Delete Pod

´´´
~/Iniciativa-Devops/temperatura/conversao-temperatura-3/k8s
└──> $ kubectl delete -f pod.yaml
pod "meupod" deleted
pod "meupod-2" deleted
´´´

# Replicaset

´´´
~/Iniciativa-Devops/temperatura/conversao-temperatura-3/k8s
└──> $ kubectl apply -f replicaset.yaml
replicaset.apps/meureplicaset created

~/Iniciativa-Devops/temperatura/conversao-temperatura-3/k8s
└──> $ kubectl get replicaset
NAME            DESIRED   CURRENT   READY   AGE

~/Iniciativa-Devops/temperatura/conversao-temperatura-3/k8s
└──> $ kubectl get pods
NAME                  READY   STATUS    RESTARTS   AGE
meureplicaset-4f4zn   1/1     Running   0          71s
meureplicaset-8kvbw   1/1     Running   0          71s
meureplicaset-8pjb5   1/1     Running   0          71s
meureplicaset-9kggl   1/1     Running   0          71s
meureplicaset-bl85f   1/1     Running   0          71s
meureplicaset-bp8rs   1/1     Running   0          71s
meureplicaset-lwqq6   1/1     Running   0          71s
meureplicaset-vct58   1/1     Running   0          71s
meureplicaset-wd5jb   1/1     Running   0          71s
meureplicaset-z87t6   1/1     Running   0          71s

~/Iniciativa-Devops/temperatura/conversao-temperatura-3/k8s
└──> $ kubectl delete -f replicaset.yaml
replicaset.apps "meureplicaset" deleted

´´´

# Deployment

´´´
~/Iniciativa-Devops/temperatura/conversao-temperatura-3/k8s
└──> $ kubectl apply -f deployment.yaml
deployment.apps/meudeployment created
service/service-web created

~/Iniciativa-Devops/temperatura/conversao-temperatura-3/k8s
└──> $ kubectl get pods
NAME                            READY   STATUS    RESTARTS   AGE
meudeployment-9fb78bf46-6gx9l   1/1     Running   0          31s
meudeployment-9fb78bf46-8kd9n   1/1     Running   0          31s
meudeployment-9fb78bf46-bf8jc   1/1     Running   0          31s
meudeployment-9fb78bf46-fldzr   1/1     Running   0          31s
meudeployment-9fb78bf46-gj848   1/1     Running   0          31s
meudeployment-9fb78bf46-lndm2   1/1     Running   0          31s
meudeployment-9fb78bf46-pdhjs   1/1     Running   0          31s
meudeployment-9fb78bf46-wrcm2   1/1     Running   0          31s
meudeployment-9fb78bf46-zldqw   1/1     Running   0          31s
meudeployment-9fb78bf46-znghd   1/1     Running   0          31s

~/Iniciativa-Devops/temperatura/conversao-temperatura-3/k8s
└──> $ kubectl get deployment
NAME            READY   UP-TO-DATE   AVAILABLE   AGE
meudeployment   10/10   10           10          64s

~/Iniciativa-Devops/temperatura/conversao-temperatura-3/k8s
└──> $ kubectl get replicaset
NAME                      DESIRED   CURRENT   READY   AGE
meudeployment-9fb78bf46   10        10        10      100s

~/Iniciativa-Devops/temperatura/conversao-temperatura-3/k8s
└──> $ kubectl get pods
NAME                            READY   STATUS    RESTARTS   AGE
meudeployment-9fb78bf46-6gx9l   1/1     Running   0          118s
meudeployment-9fb78bf46-8kd9n   1/1     Running   0          118s
meudeployment-9fb78bf46-bf8jc   1/1     Running   0          118s
meudeployment-9fb78bf46-fldzr   1/1     Running   0          118s
meudeployment-9fb78bf46-gj848   1/1     Running   0          118s
meudeployment-9fb78bf46-lndm2   1/1     Running   0          118s
meudeployment-9fb78bf46-pdhjs   1/1     Running   0          118s
meudeployment-9fb78bf46-wrcm2   1/1     Running   0          118s
meudeployment-9fb78bf46-zldqw   1/1     Running   0          118s
meudeployment-9fb78bf46-znghd   1/1     Running   0          118s

´´´

# Escalabilidade

´´´
~/Iniciativa-Devops/temperatura/conversao-temperatura-3/k8s
└──> $ kubectl delete pod meudeployment-9fb78bf46-6gx9l
pod "meudeployment-9fb78bf46-6gx9l" deleted

~/Iniciativa-Devops/temperatura/conversao-temperatura-3/k8s
└──> $ kubectl get pods
NAME                            READY   STATUS    RESTARTS   AGE
meudeployment-9fb78bf46-8kd9n   1/1     Running   0          3m20s
meudeployment-9fb78bf46-bf8jc   1/1     Running   0          3m20s
meudeployment-9fb78bf46-fldzr   1/1     Running   0          3m20s
meudeployment-9fb78bf46-gj848   1/1     Running   0          3m20s
meudeployment-9fb78bf46-h9r2h   1/1     Running   0          3s
meudeployment-9fb78bf46-lndm2   1/1     Running   0          3m20s
meudeployment-9fb78bf46-pdhjs   1/1     Running   0          3m20s
meudeployment-9fb78bf46-wrcm2   1/1     Running   0          3m20s
meudeployment-9fb78bf46-zldqw   1/1     Running   0          3m20s
meudeployment-9fb78bf46-znghd   1/1     Running   0          3m20s

´´´

´´´
~/Iniciativa-Devops/temperatura/conversao-temperatura-3/k8s
└──> $ kubectl port-forward pod/meudeployment-9fb78bf46-8kd9n 8080:80
Forwarding from 127.0.0.1:8080 -> 80
Forwarding from [::1]:8080 -> 80
´´´

# Test

http://localhost:8080/

# Troca de Versão do Blue para Green

´´´
~/Iniciativa-Devops/temperatura/conversao-temperatura-3/k8s
└──> $ watch 'kubectl get po'

NAME                            READY   STATUS              RESTARTS   AGE
meudeployment-9fb78bf46-8kd9n   1/1     Terminating         0          11m
meudeployment-9fb78bf46-fldzr   1/1     Terminating         0          11m
meudeployment-9fb78bf46-lndm2   1/1     Running             0          11m
meudeployment-9fb78bf46-pdhjs   1/1     Running             0          11m
meudeployment-9fb78bf46-wrcm2   1/1     Terminating         0          11m
meudeployment-9fb78bf46-zldqw   1/1     Terminating         0          11m
meudeployment-9fb78bf46-znghd   1/1     Running             0          11m
meudeployment-b9548cb65-5lpt7   0/1     ContainerCreating   0          1s
meudeployment-b9548cb65-75ng4   1/1     Running             0          7s
meudeployment-b9548cb65-cnjd5   1/1     Running             0          7s
meudeployment-b9548cb65-gxcvp   0/1     ContainerCreating   0          0s
meudeployment-b9548cb65-k2cdp   0/1     ContainerCreating   0          6s
meudeployment-b9548cb65-rmvkc   1/1     Running             0          7s
meudeployment-b9548cb65-spn2q   1/1     Running             0          7s

´´´
* No outro terminal:

´´´
~/Iniciativa-Devops/temperatura/conversao-temperatura-3/k8s
└──> $ kubectl apply -f deployment.yaml
deployment.apps/meudeployment configured
service/service-web unchanged
´´´

´´´
┌─[orbite]@[afya]:~/Iniciativa-Devops/temperatura/conversao-temperatura-3/k8s
└──> $ kubectl port-forward pod/meudeployment-b9548cb65-75ng4 8080:80
Forwarding from 127.0.0.1:8080 -> 80
Forwarding from [::1]:8080 -> 80
Handling connection for 8080

http://localhost:8080/

´´´

# Ele mantem meu replicaset antigo e novo

´´´
~/Iniciativa-Devops/temperatura/conversao-temperatura-3/k8s
└──> $ kubectl get replicaset
NAME                      DESIRED   CURRENT   READY   AGE
meudeployment-9fb78bf46   0         0         0       15m
meudeployment-b9548cb65   10        10        10      3m56s

´´´

# Como fazer um rollback

´´´
~/Iniciativa-Devops/temperatura/conversao-temperatura-3/k8s
└──> $ kubectl rollout history deployment meudeployment
deployment.apps/meudeployment 
REVISION  CHANGE-CAUSE
1         <none>
2         <none>

~/Iniciativa-Devops/temperatura/conversao-temperatura-3/k8s
└──> $ kubectl rollout undo deployment meudeployment
deployment.apps/meudeployment rolled back

´´´
´´´
watch 'kubectl get po'

NAME                            READY   STATUS    RESTARTS   AGE
meudeployment-9fb78bf46-48rmf   1/1     Running   0          26s
meudeployment-9fb78bf46-dsvtk   1/1     Running   0          27s
meudeployment-9fb78bf46-jhfkk   1/1     Running   0          28s
meudeployment-9fb78bf46-kgnsw   1/1     Running   0          28s
meudeployment-9fb78bf46-qjqnc   1/1     Running   0          28s
meudeployment-9fb78bf46-tm4tf   1/1     Running   0          28s
meudeployment-9fb78bf46-v5zvg   1/1     Running   0          25s
meudeployment-9fb78bf46-x729f   1/1     Running   0          27s
meudeployment-9fb78bf46-xznfc   1/1     Running   0          25s
meudeployment-9fb78bf46-zbxct   1/1     Running   0          28s

~/Iniciativa-Devops/temperatura/conversao-temperatura-3/k8s
└──> $ kubectl get replicaset
NAME                      DESIRED   CURRENT   READY   AGE
meudeployment-9fb78bf46   10        10        10      20m
meudeployment-b9548cb65   0         0         0       8m53s
´´´

´´´
~/Iniciativa-Devops/temperatura/conversao-temperatura-3/k8s
└──> $ kubectl apply -f deployment.yaml
deployment.apps/meudeployment configured
service/service-web unchanged

~/Iniciativa-Devops/temperatura/conversao-temperatura-3/k8s
└──> $ kubectl get replicaset
NAME                      DESIRED   CURRENT   READY   AGE
meudeployment-9fb78bf46   0         0         0       21m
meudeployment-b9548cb65   10        10        10      10m
´´´

# Service

´´´
~/Iniciativa-Devops/temperatura/conversao-temperatura-3/k8s
└──> $ kubectl get services
NAME          TYPE        CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE
kubernetes    ClusterIP   10.43.0.1     <none>        443/TCP        83m
service-web   NodePort    10.43.41.59   <none>        80:30393/TCP   48m
´´´

´´´
~/Iniciativa-Devops/temperatura/conversao-temperatura-3/k8s
└──> $ k3d cluster list
NAME         SERVERS   AGENTS   LOADBALANCER
meucluster   3/3       3/3      true

~/Iniciativa-Devops/temperatura/conversao-temperatura-3/k8s
└──> $ k3d cluster delete meucluster

´´´

# Criando o cluster com bind de porta

´´´
:~/Iniciativa-Devops/temperatura/conversao-temperatura-3/k8s
└──> $ k3d cluster create meucluster --servers 3 --agents 3 -p "30000:30000@loadbalancer"

~/Iniciativa-Devops/temperatura/conversao-temperatura-3/k8s
└──> $ kubectl cluster-info
Kubernetes control plane is running at https://0.0.0.0:33959
CoreDNS is running at https://0.0.0.0:33959/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy
Metrics-server is running at https://0.0.0.0:33959/api/v1/namespaces/kube-system/services/https:metrics-server:https/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.


:~/Iniciativa-Devops/temperatura/conversao-temperatura-3/k8s
└──> $ docker container ls
CONTAINER ID   IMAGE                            COMMAND                  CREATED              STATUS              PORTS                                                                            NAMES
6eafe30d0a8c   ghcr.io/k3d-io/k3d-proxy:5.4.4   "/bin/sh -c nginx-pr…"   About a minute ago   Up 34 seconds       80/tcp, 0.0.0.0:30000->30000/tcp, :::30000->30000/tcp, 0.0.0.0:33959->6443/tcp   k3d-meucluster-serverlb
d0f4ad6fefc2   rancher/k3s:v1.23.8-k3s1         "/bin/k3s agent"         About a minute ago   Up 42 seconds                                                                                        k3d-meucluster-agent-2
cc631bc54087   rancher/k3s:v1.23.8-k3s1         "/bin/k3s agent"         About a minute ago   Up 42 seconds                                                                                        k3d-meucluster-agent-1
8211d6a572fb   rancher/k3s:v1.23.8-k3s1         "/bin/k3s agent"         About a minute ago   Up 42 seconds                                                                                        k3d-meucluster-agent-0
d830b458f1d4   rancher/k3s:v1.23.8-k3s1         "/bin/k3s server --t…"   About a minute ago   Up 56 seconds                                                                                        k3d-meucluster-server-2
5d83b6f5323e   rancher/k3s:v1.23.8-k3s1         "/bin/k3s server --t…"   About a minute ago   Up About a minute                                                                                    k3d-meucluster-server-1
2b6aefe91392   rancher/k3s:v1.23.8-k3s1         "/bin/k3s server --c…"   About a minute ago   Up About a minute                                                                                    k3d-meucluster-server-0

~/Iniciativa-Devops/temperatura/conversao-temperatura-3/k8s
└──> $ kubectl apply -f deployment.yaml
deployment.apps/meudeployment unchanged
service/service-web created

~/Iniciativa-Devops/temperatura/conversao-temperatura-3/k8s
└──> $ kubectl get services
NAME          TYPE        CLUSTER-IP    EXTERNAL-IP   PORT(S)        AGE
kubernetes    ClusterIP   10.43.0.1     <none>        443/TCP        5m25s
service-web   NodePort    10.43.69.14   <none>        80:30000/TCP   74s
´´´
´´´
~/Iniciativa-Devops/temperatura/conversao-temperatura-3/k8s
└──> $ kubectl delete service service-web
service "service-web" deleted

~/Iniciativa-Devops/temperatura/conversao-temperatura-3/k8s
└──> $ kubectl apply -f deployment.yaml
deployment.apps/meudeployment unchanged

:~/Iniciativa-Devops/temperatura/conversao-temperatura-3/k8s
└──> $ kubectl get services
NAME          TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
kubernetes    ClusterIP   10.43.0.1       <none>        443/TCP        7m7s
service-web   NodePort    10.43.190.139   <none>        80:30000/TCP   3s
´´´
# Teste ok

http://localhost:30000/


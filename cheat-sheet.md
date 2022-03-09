kubectl explain pod.spec.template

k config current-context
k config set-context clustername --namespace=mynamespace
k config set-context --current --namespace=$ns
k config view | grep namespace

alias k=kubectl
alias kdp='kubectl delete --force --grace-period=0 po '
alias kdr='kubectl --dry-run=client -o yaml '
alias kc='kubectl create -f '

k logs nginx | sudo tee /etc/log.txt

kubectl taint nodes node1 a=b:NoSchedule

kubectl create quota myrq --hard=cpu=1,memory=1G,pods=2 --dry-run -o yaml

kubectl run nginx --image=nginx --port=80 -n=myns --labels=x=y --serviceaccount=myuser --env=a=b --requests='cpu=100m,memory=256Mi' --limits='cpu=200m,memory=512Mi'

kubectl set image pod/nginx nginx=nginx:1.7.1

kubectl run busybox --image=busybox --rm -it  -- wget -O- 10.1.1.131:80

kubectl create deploy nginx --image=nginx --dry-run=client --record -o yaml > y

kubectl delete po nginx --grace-period=0 --force

kubectl set image deploy nginx nginx=nginx:1.7.1

kubectl set serviceaccount deploy mydeploy mysa

kubectl scale deploy nginx --replicas 3

kubectl rollout history deploy webapp --revision=7

kubectl rollout undo deploy webapp --to-revision=3

kubectl rollout status deploy nginx

kubectl autoscale deploy webapp --min=10 --max=20 --cpu-percent=85

kubectl get po -A -o wide--show-labels -l env=prod,tier=frontend,bu=finance

kubectl logs nginx-* -p

kubectl create secret generic mysecret --from-literal=password=mypass

kubectl create docker-registry NAME --docker-username=user --docker-password=password --docker-email=email [--docker-server=string] [--from-literal=key1=value1] [--dry-run=server|client|none]

kubectl create cronjob date-job --image=busybox --schedule="*/1 * * * *" -- bin/sh -c "date; echo Hello from kubernetes cluster"

kubectl label node minikube nodeName=nginxnode

kubectl annotate pod nginx-dev{1..3} name=webapp

echo -n admin > username

kubectl create secret generic mysecret2 --from-file=username

kubectl get secret mysecret2 -o yaml

echo YWRtaW4K | base64 -d

k expose deploy nginx --name=nginx-service --port=8080 --target-port=80 --type=LoadBalancer

apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx
spec:
  strategy:
    rollingUpdate:
        maxSurge: 1
        maxUnavailable: 2
  selector:
    matchLabels:
      app: nginx
  replicas: 5
  template:
    metadata:
      labels:
        app: nginx
    spec:

     serviceAccountName: sa

     tolerations:
     - key: "key"
       operator: "Equal"
       value: "value"
       effect: "NoSchedule"

      securityContext:
          runAsUser: 0
          capabilities:
            add: [SYS_TIME]

      nodeName: master

      nodeSelector:
       c:d

      affinity:
         nodeAffinity:
           requiredDuringSchedulingIgnoredDuringExecution:
             nodeSelectorTerms:
             - matchExpressions:
               - key: color
                 operator: In
                 values:
                 - blue
               - key: nodelabel
                 operator: Exists #NotIn In
      volumes:
      - name: config-vol
        configMap:
          name: mycm

      - name: secret-vol
        secret:  
          secretName: mysecret

      - name: my-vol
        hostPath:
          path: /data
      - name: my-vol2
        emptyDir: {}
      - name: task-pv-storage
        persistentVolumeClaim:
         claimName: task-pv-claim

       containers:
      - name: nginx
        image: nginx:1.14.2

      volumeMounts:
        - name: config-vol
          mountPath: /etc/config
        - name: secret-vol
          mount-path: /etc/secret

      envFrom:
        - configMapRef:
             name: mycm

        - secretRef:
            name: db-secret
      env:
          - name: ENV1
            value: myvalue

          - name: ENV2
            valueFrom:
              configMapKeyRef:
                name: mycm
                key: height
        ports:
        - containerPort: 80
 
      resources:
          requests:
             cpu: 100m
             memory: 256Mi
          limits:
             cpu: 200m
             memory: 512Mi
        livenessProbe:
          exec:
           command: [ls, myfile]
          initialDelaySeconds: 1
          periodSeconds: 5
        readinessProbe:
          httpGet:
             path: /
             port: 80
          initialDelaySeconds: 5
          periodSeconds: 3
        readinessProbe:
          tcpSocket
             host: jim
             port: 80
          initialDelaySeconds: 5
          periodSeconds: 3


apiVersion: batch/v1
kind: Job
metadata:
  name: pi-with-timeout
spec:
  completions: 2
  parallelism: 2
  backoffLimit: 5
  activeDeadlineSeconds: 100
  template:
    spec:
      containers:
      - name: pi
        image: perl
        command: ["perl",  "-Mbignum=bpi", "-wle", "print bpi(2000)"]
      restartPolicy: Never


==========================

cat /etc/os-release
kubectl cluster-info

/var/log/kubelet.log

/var/lib/kubelet/config.yaml
/etc/kubernetes/kubelet.conf
/etc/systemd/system.kubelet.services.d

service kubelet status
service kube-apiserver status
service kube-controller-manager status
service kube-scheduler status
service kube-proxy status

service kube-apiserver stop
service etcd restart
service kube-apiserver start

sudo sysctl --system
systemctl daemon-reload
systemctl status kubelet.service
systemctl restart kubelet
systemctl restart docker
systemctl status docker.status

kubectl logs kube-apiserver

/etc/kubernetes/pki

/etc/systemd/system/kubelet.service.d/kubeadm.conf
/var/lib/kubelet/config.yaml

docker ps -a
docker logs <containerid>

==========================
#for issues with worker nodes and to add static pod to worker node

k get nodes
journalctl -u kubelet
systemctl status kubelet # this lists the *.conf and config.yaml
vi /etc/kubernetes/kubelet.conf # this is for the kubeconfig access to api server
vi /var/lib/kubelet/config.yaml
add line: staticPodPath: /etc/kubernetes/manifests
mkdir manifests
# =================

#kubeadm installation

sudo apt-get update && sudo apt-get install -y apt-transport-https curl
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
cat <<EOF | sudo tee /etc/apt/sources.list.d/kubernetes.list
deb https://apt.kubernetes.io/ kubernetes-xenial main
EOF

sudo apt-get update
sudo apt-get install -y kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl

# ===================
Upgrade the master components to exact version v1.18.0

1) Drain Master node          
kubectl drain master --ignore-daemonsets

apt update
apt install kubeadm=1.18.0-00
kubeadm upgrade apply v1.18.0
apt install kubelet=1.18.0-00
kubectl uncordon master

2) Upgrade the worker nodes to exact version v1.18.0
kubectl drain node01 --ignore-daemonsets
Run linux commands ssh on node01!
apt update
a) apt install kubeadm=1.18.0-00
b) apt install kubelet=1.18.0-00
c) kubeadm upgrade node
d) exit ssh!
Make worker node Schedulable again
kubectl uncordon node01


ETCDCTL_API=3 etcdctl member list --cacert=/etc/kubernetes/pki/etcd/ca.crt --cert=/etc/kubernetes/pki/etcd/server.crt --key=/etc/kubernetes/pki/etcd/server.key --endpoints=127.0.0.1:2379

ETCDCTL_API=3 etcdctl snapshot save --cacert=/etc/kubernetes/pki/etcd/ca.crt --cert=/etc/kubernetes/pki/etcd/server.crt --key=/etc/kubernetes/pki/etcd/server.key --endpoints=127.0.0.1:2379 /tmp/snapshot.db

etcdctl snapshot save /tmp/snapshot-pre-boot.db --cacert=/etc/kubernetes/pki/etcd/ca.crt --cert=/etc/kubernetes/pki/etcd/server.crt --key=/etc/kubernetes/pki/etcd/server.key --endpoints=https://127.0.0.1:2379

kubectl get nodes -o=jsonpath='{.items[*].metadata.name}{"\n"}'
kubectl get pods-o=jsonpath='{.items[*].metadata.name}{"\n"}'
kubectl get pods -o=jsonpath='{"Name"}{"\t"}{"Start"}{"\t\t\t"}{"Image"}{"\n"}{range .items[*]}{.metadata.name}{"\t"}{.status.startTime}{"\t"}{.spec.containers[0].image}{"\n"}{end}'


openssl genrsa -out kube-controller-manager.key 2048
openssl req -new -key kube-controller-manager.key -subj "/CN=system:kube-controller-manager" -out kube-controller-manager.csr
openssl x509 -req -in kube-controller-manager.csr -CA ca.crt -CAkey ca.key -CAcreateserial -out kube-controller-manager.crt -days 1000

kubectl get deploy -n admin2406 -o=jsonpath='{"DEPLOYMENT"}{"\t"}{"CONTAINER_IMAGE"}{"\t"}{"READY_REPLICAS"}{"\t"}{"NAMESPACE"}{"\n"}{range .items[*]}{.metadata.name}{"\t"}{.spec.template.spec.containers[0].image}{"\t"}{.spec.replicas}{"\t"}{.metadata.namespace}{"\n"}{end}'

ETCDCTL_API=3 etcdctl snapshot save -h

kubectl api-resources -o wide





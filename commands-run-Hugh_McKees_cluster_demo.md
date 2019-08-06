open repo in intellij

### go to src/main/java/cluster-sharding
open Runner.java

### two yaml files
kubernetes folder


# check minishift is functioning
see setup-local-env/minishit-mac-setup2.md

# cd to the java root directory 
mvn clean package docker:build

$ oc apply -f kubernetes/akka-cluster-rolebindging.yml
role.rbac.authorization.k8s.io/pod-reader created
rolebinding.rbac.authorization.k8s.io/read-pods created

$ oc apply -f kubernetes/akka-cluster-deployment.yml
deployment.extensions/akka-cluster-demo created
$ oc get pods
NAME                                 READY     STATUS    RESTARTS   AGE
akka-cluster-demo-56c6c46cb4-2q5w6   1/1       Running   0          8s
akka-cluster-demo-56c6c46cb4-8rhbt   1/1       Running   0          8s
akka-cluster-demo-56c6c46cb4-l4qhw   1/1       Running   0          8s



### create the service

➜  akka-java-cluster-openshift git:(master) ✗
oc expose deployment/akka-cluster-demo --type=NodePort --port 8080
service/akka-cluster-demo exposed

➜  akka-java-cluster-openshift git:(master) ✗ oc get services/akka-cluster-demo
NAME                TYPE       CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
akka-cluster-demo   NodePort   172.30.24.170   <none>        8080:31645/TCP   30s
➜  akka-java-cluster-openshift git:(master) ✗ minishift ip
192.168.99.100
➜  akka-java-cluster-openshift git:(master) ✗

### from above get the ip:port and browse to it.
http://192.168.99.100:31645



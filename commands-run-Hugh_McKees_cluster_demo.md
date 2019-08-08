# Steps to run, build and deploy Akka Cluster crop circle
browse to and open Hugh's repo https://github.com/mckeeh3/akka-java-cluster-openshift

 in intelliJ as maven project


## Study the Java files 
### go to src/main/java/cluster-sharding
open Runner.java

### Note the two yaml files
in the kubernetes folder


# check to see if minishift is functioning 
see [minishift setup on Mac](setup-local-env/minishit-mac-setup2.md)

# cd to the java root directory 
 open the cli and run maven to build the project 

 ``` sh
cd <<root folder>>
$> mvn clean package docker:build



 ```

and deploy to `minishift`

```sh

$> oc apply -f kubernetes/akka-cluster-rolebindging.yml
role.rbac.authorization.k8s.io/pod-reader created
rolebinding.rbac.authorization.k8s.io/read-pods created

$> oc apply -f kubernetes/akka-cluster-deployment.yml
deployment.extensions/akka-cluster-demo created

$> oc get pods
NAME                                 READY     STATUS    RESTARTS   AGE
akka-cluster-demo-56c6c46cb4-2q5w6   1/1       Running   0          8s
akka-cluster-demo-56c6c46cb4-8rhbt   1/1       Running   0          8s
akka-cluster-demo-56c6c46cb4-l4qhw   1/1       Running   0          8s

```

### create the service as well as open the public ip ( route )

```sh


$> oc expose deployment/akka-cluster-demo --type=NodePort --port 8080
service/akka-cluster-demo exposed

$> oc get services/akka-cluster-demo

 NAME                TYPE       CLUSTER-IP      EXTERNAL-IP   PORT(S)          AGE
akka-cluster-demo   NodePort   172.30.24.170   <none>        8080:31645/TCP   30s

$> minishift ip 192.168.99.100
```

### from above get the ip:port and browse to it.
### for example
http://192.168.99.100:31645



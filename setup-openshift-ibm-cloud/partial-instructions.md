``` sh
#prev commands

#login get token top right hand OKD/openshift web console - from pull down `copy login command` choice

oc login https://c100-e.us-south.containers.cloud.ibm.com:32627 --token=N-umsJh9_xJA-LI0aPZkrNdG-ACU0dRQ0oBNEbZlssw

#create project
oc new-project akka-cluster-1 --description="Akka Java Cluster Kubernetes Example" --display-name="akka-cluster-1"

#view projects
oc get projects

#switch to right project
oc project akka-cluster-1
```

### BUILD
```
mvn clean package docker:build
```

## EXTRA STEPS: I had to push docker image to docker hub account I have

```sh
docker images
#TAG docker image
docker tag akka-cluster-demo:1.0.0 grantsteinfeldibm/akka-cluster-demo:1.0.0
docker images
docker push grantsteinfeldibm/akka-cluster-demo:1.0.0

```
## note!! THEN
### change kubernetes/akka-cluster-deployment.yml
from imagePullPolicy: `Never` to imagePullPolicy: `Always`
and also change the repo to pull to include your docker hub usernameso in my file ll. 27-28

```
image: grantsteinfeldibm/akka-cluster-demo:1.0.0
imagePullPolicy: Always
```


#### deploy docker images

```sh
oc apply -f kubernetes/akka-cluster-deployment.yml
```

### I created route on openshift web console not via command line
e.g. https://c100-e.us-south.containers.cloud.ibm.com:32627/console/catalog
in my case openshift instance on dev adv cloud console

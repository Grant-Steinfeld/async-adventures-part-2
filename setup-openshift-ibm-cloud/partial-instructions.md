docker images

docker tag akka-cluster-demo:1.0.0 grantsteinfeldibm/akka-cluster-demo:1.0.0
docker images
docker push grantsteinfeldibm/akka-cluster-demo:1.0.0


#note!!
#change kubernetes/akka-cluster-deployment.yml
# imagePullPolicy: Never
# to
# imagePullPolicy: Always
#deploy docker images$ 

oc apply -f kubernetes/akka-cluster-deployment.yml

I created route on openshift web console 

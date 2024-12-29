#Set the following environment variables to the name and namespace where the Istio ingress gateway is located in your cluster:

export INGRESS_NAME=istio-ingressgateway
export INGRESS_NS=istio-system

#Run the following command to determine if your Kubernetes cluster is in an environment that supports external load balancers:

**$ kubectl get svc "$INGRESS_NAME" -n "$INGRESS_NS"
NAME                   TYPE           CLUSTER-IP       EXTERNAL-IP      PORT(S)   AGE
istio-ingressgateway   LoadBalancer   172.21.109.129   130.211.10.121   ...       17h
**
**export INGRESS_HOST=$(kubectl -n "$INGRESS_NS" get service "$INGRESS_NAME" -o jsonpath='{.status.loadBalancer.ingress[0].hostname}')**
**export INGRESS_PORT=$(kubectl -n "$INGRESS_NS" get service "$INGRESS_NAME" -o jsonpath='{.spec.ports[?(@.name=="http2")].port}')**
**export SECURE_INGRESS_PORT=$(kubectl -n "$INGRESS_NS" get service "$INGRESS_NAME" -o jsonpath='{.spec.ports[?(@.name=="https")].port}')**
**export TCP_INGRESS_PORT=$(kubectl -n "$INGRESS_NS" get service "$INGRESS_NAME" -o jsonpath='{.spec.ports[?(@.name=="tcp")].port}')**

#for ingress_host_ip
**export INGRESS_HOST=$(kubectl -n "$INGRESS_NS" get service "$INGRESS_NAME" -o jsonpath='{.status.loadBalancer.ingress[0].ip}')**

# to get application URL
**export GATEWAY_URL=$INGRESS_HOST:$INGRESS_PORT**


#Confirm the app is accessible from outside the cluster
To confirm that the Bookinfo application is accessible from outside the cluster, run the following curl command:

$ **curl -s "http://${GATEWAY_URL}/productpage" | grep -o "<title>.*</title>"**
<title>Simple Bookstore App</title>**

You can also point your browser to **http://$GATEWAY_URL/productpage** to view the Bookinfo web page. If you refresh the page several times, you should see different versions of reviews shown in productpage, presented in a round robin style (red stars, black stars, no stars), since we haven’t yet used Istio to control the version routing.


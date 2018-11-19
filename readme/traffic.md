# Istio on GKE

<img src="https://cdn-images-1.medium.com/max/2000/1*Z_-ulLqHoVA2jOVIEU3G5Q.png" height="250" width="1000"/>

[![Setting Up GKE Cluster](https://github.com/nikitsrj/gdg-istio/blob/master/readme/setupgke.png)](./agenda.md)[![Installing Istio](https://github.com/nikitsrj/gdg-istio/blob/master/readme/istioinstall.png)](./istio.md)[![Traffic Management](https://github.com/nikitsrj/gdg-istio/blob/master/readme/trafficenable.png)](./traffic.md)[![Security Authentication](https://github.com/nikitsrj/gdg-istio/blob/master/readme/authentication.png)](./agenda.md)[![Telemetry](https://github.com/nikitsrj/gdg-istio/blob/master/readme/telem.png)](./agenda.md)

## Traffic Management

This section of demo contains basically three different aspects of traffic management.

1. Access a specific version of a service
2. Path Based routing (You can relate to Nginx Ingress Controller :) )
3. Intelligent Routing (Weight Based) for canary Deployment

To checkout all the above features, we need to deploy an application service. Follow the below steps to deploy a sample app.

### Deploy Nova Service (Sample App)

1. Clone the below git url to Cloud Shell
```
git clone https://github.com/nikitsrj/gdg-istio.git
```

2. Navigate to the `trafficRouting` folder and hit below command to deploy the application and its service 
```
cd gdg-istio/trafficRouting/
kubectl create -f nova-v1.yaml
kubectl create -f nova-svc.yaml
```

3. Now we need to create a `Gateway` to enable HTTP/HTTPS traffic to our service mesh
```
kubectl create -f nova-gateway.yaml
```
4. Post that we have to map the Kubernetes Service with the Gateway using a `VirtualService`.
```
kubectl create -f nova-virtualService.yaml
```
5. Now test `v1` of the application by hitting the `EXTERNAL-IP` in the browser (or Postman), that we will recieve by entering below command in cloud shell
```
kubectl get svc istio-ingressgateway -n istio-system
```

#### Deploying multiple version of the same service 

If you want to update your app to a new version. Maybe you want to split the traffic between two versions. You need to create a DestinationRule to define those versions, called subsets in Istio. 

Deploy the second version of the application.
```
kubectl create -f nova-v2.yaml
```
If you refresh the browser with the `EXTERNAL-IP` , you’ll see that VirtualService rotates through `v1` and `v2` versions of the app. This is expected because both versions are exposed behind the same Kubernetes service: `nova-svc`.

### Access a specific version of a service
If you want to pin your service to only v2, so this can be done by specifying a subset in the `VirtualService` but we need to define those subsets first in a `DestinationRules`. A DestinationRule essentially maps labels to Istio subsets.

1. Create the destination Rule.
```
kubectl create -f nova-destinationRule.yaml
```
2. Now you can modify the VirtualService file to point to v2 of the application. So it will look like this.
```
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
  name: nova-virtualservice
spec:
  hosts:
  - "*"
  gateways:
  - nova-gateway
  http:
  - route:
    - destination:
        host: nova-svc
        subset: v2
```
3. Update the VirtualService
```
kubectl apply -f nova-virtualService.yaml 
```

### Path Based Routing

If you need something like path based routing then also me need to prefix in match uri section of VirtualService file like below.

```
apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
  name: nova-virtualservice
spec:
  hosts:
  - "*"
  gateways:
  - nova-gateway
  http:
  - match:
    - uri:
        prefix: /nova
    route:
    - destination:
        host: nova-svc
        subset: v1
  ```
  Now update the virtualService
  ```
  kubectl create -f nova-virtualService.yaml
  ```
  Now open the browser (or Postman) and hit the  `EXTERNAL-IP/nova` , you will get the page of v1 of the application.
  
  ### Weight Based Routing (Canary Deployment)
  
  In case you need canary deployment of the application and want the traffic ratio of 80% in v1 of the app and 20% in v2 of the app, then we have to do the below changes.
```
   apiVersion: networking.istio.io/v1alpha3
kind: VirtualService
metadata:
  name: nova-virtualservice
spec:
  hosts:
  - "*"
  gateways:
  - nova-gateway
  http:
  - route:
    - destination:
        host: nova-svc
        subset: v1
      weight: 80
    - destination:
        host: nova-svc
        subset: v2
      weight: 20
 ```
 Now update the virtualService
  ```
  kubectl create -f nova-virtualService.yaml
  ```
 Now open the browser (or Postman) and hit the  `EXTERNAL-IP`, most of the time you will be redirected to v1 of the app and sometimes v2.
 
  
  


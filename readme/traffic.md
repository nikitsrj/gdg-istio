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
4. Now we have to map the Kubernetes Service with the Gateway using a `VirtualService`.
```
kubectl create -f nova-virtualService.yaml
```


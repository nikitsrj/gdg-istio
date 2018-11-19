# Istio on GKE

<img src="https://cdn-images-1.medium.com/max/2000/1*Z_-ulLqHoVA2jOVIEU3G5Q.png" height="250" width="1000"/>

[![Setting Up GKE Cluster](https://github.com/nikitsrj/gdg-istio/blob/master/readme/setupgke.png)](./agenda.md) [![Installing Istio](https://github.com/nikitsrj/gdg-istio/blob/master/readme/enableistio.png)](./istio.md) [![Traffic Management](https://github.com/nikitsrj/gdg-istio/blob/master/readme/traffic.png)](./agenda.md)[![Security Authentication](https://github.com/nikitsrj/gdg-istio/blob/master/readme/authentication.png)](./agenda.md)[![Telemetry](https://github.com/nikitsrj/gdg-istio/blob/master/readme/telem.png)](./agenda.md)

## Installing Istio

Now let's install Istio. Starting with the 0.2 release, Istio is installed in its own istio-system namespace, and can manage microservices from all other namespaces. The installation includes Istio core components, tools, and samples.

Follow the below steps in the cloud shell

1. Download the Istio installation files according to your OS. Here we are taking Linux cause Cloud Shell is based on Debian
```
wget https://github.com/istio/istio/releases/download/1.0.3/istio-1.0.3-linux.tar.gz
```
2. Extract the files and go to the istio directory and make the istioctl binary global
```
tar -xvf istio-1.0.3-linux.tar.gz
cd istio-1.0.3
sudo cp bin/istioctl /usr/local/bin
```
3. Now it's time to install Istio core components with mutual authentication between sidecars
```
kubectl apply -f install/kubernetes/istio-demo-auth.yaml
```




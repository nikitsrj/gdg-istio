# Istio on GKE

<img src="https://cdn-images-1.medium.com/max/2000/1*Z_-ulLqHoVA2jOVIEU3G5Q.png" height="250" width="1000"/>

[![Setting Up GKE Cluster](https://github.com/nikitsrj/gdg-istio/blob/master/readme/setupgke.png)](./agenda.md)[![Installing Istio](https://github.com/nikitsrj/gdg-istio/blob/master/readme/istioinstall.png)](./agenda.md)[![Traffic Management](https://github.com/nikitsrj/gdg-istio/blob/master/readme/traffic.png)](./agenda.md)[![Security Authentication](https://github.com/nikitsrj/gdg-istio/blob/master/readme/authentication.png)](./agenda.md)[![Telemetry](https://github.com/nikitsrj/gdg-istio/blob/master/readme/telem.png)](./agenda.md)


## Setting up GKE Cluster

### Before you begin

Take the following steps to enable the Kubernetes Engine API:
1. Visit the [Kubernetes Engine page](https://console.cloud.google.com/projectselector/kubernetes?_ga=2.190316075.-1430225704.1540446362) in the Google Cloud Platform Console.
2. Create or select a project.
3. Wait for the API and related services to be enabled. This can take several minutes.
4. Make sure that billing is enabled for your project.

### Fireup CloudShell and configure it
Once the project creation completed then open Cloudshell, which have some preinstalled packages like docker, git, gcloud, and perform below operation.

1. Install Kubectl into the cloudshell
```
gcloud components install kubectl
```
2. Setup defaults config for gcloud
```
gcloud config set project [PROJECT_ID]
gcloud config set compute/zone us-central1-b
```
### Create a cluster
1. To create a cluster for this demo, run the following command - let's call the demo cluster (It will take 3-4min to create the cluster)

```
gcloud container clusters create istio-demo \
    --machine-type=n1-standard-1 \
    --num-nodes=4 \
    --no-enable-legacy-authorization
```

# Istio on GKE

<img src="https://cdn-images-1.medium.com/max/2000/1*Z_-ulLqHoVA2jOVIEU3G5Q.png" height="250" width="1000"/>

[![Setting Up GKE Cluster](https://github.com/nikitsrj/gdg-istio/blob/master/readme/setupgke.png)](./agenda.md)[![Installing Istio](https://github.com/nikitsrj/gdg-istio/blob/master/readme/istioinstall.png)](./istio.md)[![Traffic Management](https://github.com/nikitsrj/gdg-istio/blob/master/readme/traffic.png)](./traffic.md)[![Security Authentication](https://github.com/nikitsrj/gdg-istio/blob/master/readme/authentication.png)](./security.md)[![Telemetry](https://github.com/nikitsrj/gdg-istio/blob/master/readme/enabletelemetry.png)](./telemetry.md)

## Telemetry

This section of demo contains below things.
1. Distributed Tracing via Jaeger UI
2. Visualizing Metrics with Grafana
3. Querying Metrics from Prometheus
4. Generating a Service Graph
5. Logging with Fluentd and visualizing via Kibana

### Distributed Tracing via Jaeger UI

We can collect trace spans of istio-enabled applications via Jaeger UI. By default Istio already provides the Jaeger application deployed and configured with mixer. To access the dashboard hit the below command in Cloud Shell. 

```
kubectl port-forward --namespace istio-system $(kubectl get pod --namespace istio-system --selector="app=jaeger" --output jsonpath='{.items[0].metadata.name}') 8084:16686
```

Post that click on `web preview` of cloud shell and change the port to 8084 and Preview it. You will get to see the dashboard like below.
<img src="https://github.com/nikitsrj/gdg-istio/blob/master/readme/Screenshot%202018-11-20%20at%205.55.06%20PM.png"/>
<img src="https://github.com/nikitsrj/gdg-istio/blob/master/readme/Screenshot%202018-11-20%20at%205.58.27%20PM.png"/>

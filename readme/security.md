# Istio on GKE

<img src="https://cdn-images-1.medium.com/max/2000/1*Z_-ulLqHoVA2jOVIEU3G5Q.png" height="250" width="1000"/>

[![Setting Up GKE Cluster](https://github.com/nikitsrj/gdg-istio/blob/master/readme/setupgke.png)](./agenda.md)[![Installing Istio](https://github.com/nikitsrj/gdg-istio/blob/master/readme/istioinstall.png)](./istio.md)[![Traffic Management](https://github.com/nikitsrj/gdg-istio/blob/master/readme/traffic.png)](./traffic.md)[![Security Authentication](https://github.com/nikitsrj/gdg-istio/blob/master/readme/enabledsecurity.png)](./security.md)[![Telemetry](https://github.com/nikitsrj/gdg-istio/blob/master/readme/telem.png)](./telemetry.md)

## Security

This section of demo have example of `End-User Authentication`. To experiment with this feature, you need a valid JWT. The JWT must correspond to the JWKS endpoint you want to use for the demo. In this tutorial, we use this [JWT](https://raw.githubusercontent.com/istio/istio/release-1.0/security/tools/jwt/samples/demo.jwt) test and this [JWKS](https://raw.githubusercontent.com/istio/istio/release-1.0/security/tools/jwt/samples/jwks.json) endpoint from the Istio code base.

We will be using same Nova Application to test the authentication feature. Right now our application is working completely fine and anyone can access it without any authentication. To enforce the authentication we have to write a policy and attach the `nova-svc` as its target, so that the policy should get enforced whenever anyone tries to access the `nova-svc`.

The policy will look like below.
```
apiVersion: "authentication.istio.io/v1alpha1"
kind: "Policy"
metadata:
  name: "nova-auth-policy"
spec:
  peers:
  - mtls: {}
  targets:
  - name: nova-svc
  origins:
  - jwt:
      issuer: "testing@secure.istio.io"
      jwksUri: "https://raw.githubusercontent.com/istio/istio/release-1.0/security/tools/jwt/samples/jwks.json"
  principalBinding: USE_ORIGIN
  ``` 
  
Create the policy.
```
kubectl create -f nova-authPolicy.yaml
```

Now hit below command in your cloud shell, you will get `401`.
```
curl http://EXTERNAL-IP -s -o /dev/null -w "%{http_code}\n" 
```
But if you pass the bearer token then you will get `200`
```
TOKEN=$(curl https://raw.githubusercontent.com/istio/istio/release-1.0/security/tools/jwt/samples/demo.jwt -s)
curl --header "Authorization: Bearer $TOKEN" http://EXTERNAL-IP -s -o /dev/null -w "%{http_code}\n"
```

[![Next: Telemetry](https://github.com/nikitsrj/gdg-istio/blob/master/readme/nexttelemetry.png)](./telemetry.md)

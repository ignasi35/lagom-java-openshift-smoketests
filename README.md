# Lagom Minimal Deployment Sample

This is a minimal Lagom application used to demo and smoke-test deployment on Kubernetes and Openshift.

### Components

1. a `hello` service with a single endpoint responding with an echo
1. the `hello` service is setup to require 3 nodes and build an Akka Cluster
1. a `hello-proxy` service with a single endpoint forwarding the request to `hello` service

The above will test:

* cluster bootstrap to startup `hello` (PENDING)
* service discovery for intra-service (during cluster bootstrap)
* service discovery for inter-service (from `hello-proxy` to `proxy`)

**NOTE**: this minimal sample avoids using any DB or broker to reduce the resources required to run. 

## Running in `minikube`

Start `minikube` and setup your terminal:

```bash
minikube start
export MINIKUBE_IP=`minikube ip`
eval $(minikube docker-env)
```

Deploy the application: 

```bash
mvn package docker:build 
kustomize build deployment/overlays/minikube | kubectl apply -f - 
```

Test the deployment:

```bash
curl https://$MINIKUBE_IP/proxy/rest-hello/alice
curl https://$MINIKUBE_IP/api/hello/alice
minikube dashboard  ## go to Deployments and scale each service at will
```

## How this works

TODO

## Sample license

Written in 2019 by Lightbend, Inc.

To the extent possible under law, the author(s) have dedicated all copyright and related
and neighboring rights to this template to the public domain worldwide.
This template is distributed without any warranty. See <http://creativecommons.org/publicdomain/zero/1.0/>.

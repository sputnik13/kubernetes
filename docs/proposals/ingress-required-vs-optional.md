# Ingress and optional capabilities

## Abstract

[Ingress Resource](http://kubernetes.io/docs/user-guide/ingress/) is currently
a beta resource in Kubernetes with support for definition of L7 Ingress.

Ingress as a resource is a useful concept to be able to define and manage
how Services are exposed to entities outside of the Kubernetes cluster.
However, the capabilities available in each cloud provider vary quite a bit as 
discussed in [Current Support in Cloud Provider Load Balancers](## Current Support in Cloud Provider Load Balancers).

Given the asymetry of capabilities across cloud providers, the Ingress Resource
can provide either the lowest common denominator or support the notion of
optional capabilities for Ingress controllers.

## Motivation

## Current Support by Cloud Provider Load Balancers

### L7 (HTTP/HTTPS) Support

|           | HTTP | HTTPS | Path based | Virtual host | SNI |
| --------- | ---- | ----- | ---------- | ------------ | --- |
| AWS       |   x  |   x   |            |              |     |
| GCE       |   x  |   x   |      x     |       x      |  x  |
| Mesos     |   ?  |   ?   |      ?     |       ?      |  ?  |
| Openstack |   x  |   x   |      x     |       x      |  x  |
| Rackspace |      |       |            |              |     |
| vSphere   |   x  |   x   |      x     |       x      |  x  |

*Taken from [Ingress HTTP](ingress-http.md)*

### L4 Protocol Support

|           | TCP | UDP | TLS |
| --------- | --- | --- | --- |
| AWS       |  x  |     |  x  |
| GCE       |  x  |  x  |     |
| Mesos     |  ?  |  ?  |  ?  |
| Openstack |  x  |  x  |  x  |
| Rackspace |  x  |  x  |     |
| vSphere   |  x  |  x  |  x  |

*Taken from [Ingress L4 Proposal](ingress-l4.md)*

## Use Cases
## Design

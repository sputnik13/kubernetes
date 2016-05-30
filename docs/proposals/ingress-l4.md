# Ingress L4 Proposal

## Abstract

A proposal to add support for TCP and UDP to the Ingress Resource.

Existing issue regarding Ingress and TCP
* Ingress and TCP [#23291](https://github.com/kubernetes/kubernetes/issues/23291)


## Use Cases

1. Be able to accept and route traffic for arbitrary TCP ports
1. Be able to accept and route traffic for arbitrary UDP ports


## Motivation

When deploying and exposing non-http(s) based services, Kubernetes users are
limited to NodePort and service type=LoadBalancer as methods for exposing these
services using interfaces supported by the Kubernetes API.

Other methods exist, via contributed code, for exposing non-http(s) based services:

* [keepalived-vip](https://github.com/kubernetes/contrib/tree/master/keepalived-vip)
* NGINX Ingress Controller
  [nginx-ingress](https://github.com/kubernetes/contrib/tree/master/ingress/controllers/nginx)

However these implementations rely on a ConfigMap being populated with a
mapping of destination endpoint to service.  A single common interface for
describing all Ingress would is highly desirable.


## Design

### TCP and UDP support

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
    name: test-ingress
spec:
    rules:
    - tcp:
        port: 53
      backend:
        serviceName: testdns
        servicePort: 53
    - udp:
        port: 53
      backend:
        serviceName: testdns
        servicePort: 53
```

While HTTP rules have host specifiers, in TCP and UDP rules they would not be
allowed as hostname handling is specific to L5 or higher layer protocol being
used over TCP or UDP.  TCP and UDP as protocol specifiers should be agnostic to
higher layer protocols.

### TCP with TLS

Given the following secret

```yaml
apiVersion: v1
kind: Secret
data:
    tls.crt: base64 encoded cert
    tls.key: base64 encoded key
metadata:
    name: testsecret
type: Opaque
```

TCP with TLS would be almost identical to the existing TLS Ingress
specification with the addition of the tcp attribute for the Ingress rule

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
    name: test-ingress
spec:
    rules:
    - tcp:
        port: 3306
      host: www.foo.com
      tls:
        - secretName: testsecret
      backend:
        serviceName: testmysql
        servicePort: 3306
```

TLS as a protocol does support hostnames via SNI, thus allowing a host
specifier seems to make sense and opens the possibility of supporting multiple
TCP services on a single IP.

### Ingress type specifier

The current Ingress API assumes a protocol based on the presence or absence
of a key (tls, http).  This leaves open the possibility of a spec that defines
all protocols.

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
    name: test-ingress
spec:
    rules:
    - tcp:
        port: 3306
      udp:
        port: 3306
      host: www.foo.com
      tls:
        - secretName: testsecret
      backend:
        serviceName: testmysql
        servicePort: 3306
      http:
        paths:
        - path: /bar
          backend:
              serviceName: bar
              servicePort: 80
```

A specification like the one above is arguably too complex and should be
written as separate rules for each protocol being supported.  However, the
loose syntax of the specification allows construction of a complex
specification.  Using a 'type' or 'protocol' specifier would remove this
ambiguity in the specification.

```yaml
apiVersion: extensions/v1beta1
kind: Ingress
metadata:
    name: test-ingress
spec:
    rules:
    - protocol: tcp
      port: 3306
      backend:
        serviceName: testmysql
        servicePort: 3306

    - protocol: udp
      port: 3306
      backend:
        serviceName: testmysql
        servicePort: 3306

    - protocol: tls
      host: www.foo.com
      tls:
        - secretName: testsecret
      http:
        paths:
        - path: /bar
          backend:
              serviceName: bar
              servicePort: 80
```


## State of L4 Protocol Support in Cloud Providers

### Summary

|           | TCP | UDP | TLS |
| --------- | --- | --- | --- |
| AWS       |  x  |     |  x  |
| GCE       |  x  |  x  |     |
| Mesos     |  ?  |  ?  |  ?  |
| Openstack |  x  |  x  |  x  |
| Rackspace |  x  |  x  |     |
| vSphere   |  x  |  x  |  x  |

### AWS

#### Protocols

 - HTTP
 - HTTPS
 - TCP
 - TLS

#### Security Groups

Required for each LB instance

#### Static IP

No support for static IP

### GCE

#### Protocols

 - HTTP
 - HTTPS
 - TCP
 - UDP

### Mesos

### Openstack

 - TCP
 - UDP

### Rackspace

 - TCP
 - UDP

The following protocols are advertised but they seem like convenience options
that map to a TCP or UDP port as appropriate, rather than actual support for
the underlying protocol.

 - DNS
 - FTP
 - HTTP
 - HTTPS
 - IMPAS
 - IMPAv2
 - IMPAv3
 - IMPAv4
 - LDAP
 - LDAPS
 - MYSQL
 - POP3
 - POP3S
 - SFTP
 - SMTP

### vSphere

Supported protocols for vSphere vary based on the load balancer appliance used
with vSphere.  An in-cluster (k8s) software load balancer is currently
available in contrib that use NGINX as the software load balancing solution.

 - HTTP
 - HTTPS
 - TCP
 - TLS
 - UDP

# Ingress Resource Sharing

## State of Load Balancer Sharing in Cloud Providers

### AWS

No static IP support

LB per IP (dynamic)

Single LB can have multiple listeners

    listener == (LB protocol, LB port, Instance protocol, Instance port)

Seems like virtual hosting (whether HTTP or HTTPS) is not supported

### GCE

Static IP support

LB per supported protocol (HTTP(S), TCP, UDP)

Single LB can have multiple listeners

    HTTP(S) listener == (hostname, path)
    TCP listener == (LB IP, LB port)
    UDP listener == (LB IP, LB port)

### Mesos

### Openstack

Static IP support

### Rackspace

### vSphere

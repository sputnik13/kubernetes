# Ingress Healthchecks

## Abstract
## Motivation
## Use Cases
## Design

## State of L4 Support in Cloud Providers

### AWS

#### Protocols

 - HTTP(S)
   - "ping" by retrieving a URI
 - TCP
   - "ping" port (most likely a TCP SYN)
 - SSL (TCP)
   - "ping" port (most likely a TCP SYN)

#### Threshold

 - Timeout (seconds)
 - Interval (seconds)
 - Unhealthy threshold - number of failures to declare unhealthy
 - Healthy threshold - number of successes to declare healthy

### GCE

#### Protocols

 - HTTP(S)
   - make a request to a path
   - supports optional host http header

#### Health criteria

 - Timeout (seconds)
 - Check interval (seconds)
 - Unhealthy threshold - number of failures to declare unhealthy
 - Healthy threshold - number of successes to declare healthy

### Mesos

### Openstack

### Rackspace

### vSphere

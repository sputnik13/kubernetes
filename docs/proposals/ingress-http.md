# Ingress L7

## State of L7 (HTTP/HTTPS) Support in Cloud Providers

### Summary

|           | HTTP | HTTPS | Path based | Virtual host | SNI |
| --------- | ---- | ----- | ---------- | ------------ | --- |
| AWS       |   x  |   x   |            |              |     |
| GCE       |   x  |   x   |      x     |       x      |  x  |
| Mesos     |   ?  |   ?   |      ?     |       ?      |  ?  |
| Openstack |   x  |   x   |      x     |       x      |  x  |
| Rackspace |      |       |            |              |     |
| vSphere   |   x  |   x   |      x     |       x      |  x  |

### AWS

No path based forwarding
No virtual hosting (neither HTTP nor HTTPS)
Supports bridging HTTPS frontend to HTTP backend and vice versa

### GCE

Path based forwarding
Virtual host support, including SNI
Supports bridging HTTPS frontend to HTTP backend and vice versa

### Mesos

### Openstack

### Rackspace

### vSphere

# How to setup a JunOS device for GNMI

The configuration is fairly simple for a lab scenario where we will not use certificates and use the insecure mode.
Please refer to the Juniper Site for details on configuration using Certificates.

This was tested on the vMX 22.3R1.11 and vQFX 19.4R1.10
Note: Although configuration is accepted on the vQFX it was not possible to get GNMI working on this platform. This could be due to the fact that the insecure mode is not supported in earlier releases 

```
set system services extension-service request-response grpc clear-text port 57400
set system services extension-service request-response grpc max-connections 4
```


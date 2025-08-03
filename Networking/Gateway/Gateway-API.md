# Gateway-API

-> supports Ingress, Load Balancing and Service Mesh APIs

GatewayClass - defines the waht the underlying network infrastructure would be such as Nginx, traefik or other Load balancer

Cluster Operators - configure the Gateway, which are the instances of the Gateway class

HttpRoute - Created by the application developers

Like HttpRoute, we have TCPRoute, GRPCRoute

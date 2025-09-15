## kubernetes-gateway-api
Exercise repository to understand how to use gateway-api feature in Kubernetes


### What is the Gateway API?

- The Gateway API is a next-generation, Kubernetes-native API for managing network traffic into and within clusters.

- Think of it as the evolution of Ingress, designed to overcome the limitations of Ingress and Service APIs.

- It provides a more expressive, extensible, and role-oriented model for routing traffic.

### Why Gateway API (vs Ingress)?

#### Ingress (the old way):

- Limited to HTTP/HTTPS mainly.

- Configuration was often provider-specific (annotations, custom configs).

- Harder to support advanced use cases (e.g., traffic splitting, mTLS, TCP/UDP).

#### Gateway API (the new way):

- Works not just for HTTP, but also for TCP, UDP, gRPC, WebSockets, etc.

- Portable across implementations (NGINX, Istio, HAProxy, Cilium, etc.).

- Role-oriented resources: separates responsibilities between infra teams and app teams.

- Extensible: Vendors can add their own features without breaking portability.

#### Key Concepts & Resources

- Gateway API introduces several new Kubernetes resources:

    - **GatewayClass**

        - Defines a class of gateways (like IngressClass for Ingress).

        Example: "nginx", "istio", "gke-l7-global-external-managed".

    - **Gateway**

        - A request for a specific instance of a data-plane (load balancer, proxy).

        - Think of it like: “I want an external IP or load balancer using the nginx GatewayClass.”

    - **HTTPRoute** (or TCPRoute, UDPRoute, GRPCRoute)

        - Defines how traffic is routed.

        Example: “When a request comes to /foo, send it to Service A. /bar, send it to Service B.”

     - **Route Binding**

        - Routes bind to Gateways (1 Gateway can serve multiple Routes).

        - This allows multi-tenancy and team isolation.

### Usage Pattern

- Infra provision team defines the Gateway Class

- DevOps team: defines Gateways (control IPs, TLS, LB policies).

- Application team: defines Routes (what traffic goes where).

- Cleaner separation of concerns.

- Works across vendors (NGINX, Istio, Cilium, GKE, etc.).
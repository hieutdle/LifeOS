## nginx as a web server

![[What is nginx-1783614285654.webp]]

![[What is nginx-1783614348439.webp]]

## nginx as a proxy server

![[What is nginx-1783614394434.webp]]


![[What is nginx-1783614480294.webp]]

But load balancing is just one of thefunctionality of nginx proxy server

![[What is nginx-1783614462273.webp]]

Caching

![[What is nginx-1783614540769.webp]]

![[What is nginx-1783614575290.webp]]

You can put all your security on one server instead of multiple servers. So having one public accessible entry point reduces security surface and act like a shield/security for all remaining web server.

Also encrypted communication

![[What is nginx-1783614687217.webp]]

Compression

![[What is nginx-1783614749000.webp]]

## nginx configuration

You can configure nginx server to be web server or proxy server by configuration it should forward traffic to other web server or handle itself

![[What is nginx-1783614911839.webp]]

#### Configure Load Balancing

![[What is nginx-1783614987781.webp]]

![[What is nginx-1783615027991.webp]]

## nginx as a k8s ingress controller

Ingress controller is specialized load balancer for managing ingress (incoming traffic in k8s)

What it does with web servers, it does with service inside cluster now. It receives incoming traffic first, then based on the configuration we define forward to the right service inside the cluster.

![[What is nginx-1783615215618.webp]]

![[What is nginx-1783615287070.webp]]

Add additional security layer with cloud load balancer.





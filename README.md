# ebpf-web-proxy
HTTP proxy based on eBPF rules to redirect traffic on packet level.

Reverse proxy is used to hide multiple web services with one URL domain.
For example on website `example.com` may be you may have API service `example.com/api/`, frontend `example.com/` and blog `example.com/blog/`.
So you may set up a reverse proxy that will receive all requests on example.com but internally redirects them to a needed service assiciated with URL path.


## Current solutions
* [Apache HTTPD mod_proxy](https://httpd.apache.org/docs/2.4/mod/mod_proxy.html) is a default solution for this.
* [NGINX](https://nginx.org/) most popular reverse proxy and web server for static files focused on performance. Used by most CDNs.
* [HAProxy](https://www.haproxy.org/) most advanced proxy for high load services.
* [Traefic](https://traefik.io/) and [Caddy](https://caddyserver.com) are reverse proxies for midle size projects with microservices.

And some small solutions:
* [Lighttpd mod_proxy](https://redmine.lighttpd.net/projects/lighttpd/wiki/Docs_ModProxy)
* [TinyProxy](https://tinyproxy.github.io/)
* [OpenBSD relayd](https://man.openbsd.org/relayd.8)
* [BusyBox httpd](https://openwrt.org/docs/guide-user/services/webserver/http.httpd) may be compiled with a very basic reverse proxy. The smallest solution.

## Why eBPF is needed here?
The problem is that they all work on L4 and this limits throughtput.
Instead we may try to make the redirection of packets dirreclty on network level.
To do that we may use eBPF rules and analyze packet.
There is a research project to analyze HTTP packets:
https://github.com/iovisor/bcc/tree/master/examples/networking/http_filter

But for HTTPS all packets are encrypted and we can't analyze them.
To solve this we may terminate TLS with stunnel and redirect to plain http port.
Also for internal communication between microservcies TLS is not needed.

* https://youtu.be/KHMnC3kj3Js
* https://github.com/cilium/cilium
* https://www.tigera.io/learn/guides/ebpf/
* https://github.com/projectcalico/calico/blob/ee73bcf9bd6f18ae686770b736ffa522d17a433d/about/about-ebpf.md
* https://github.com/netgroup-polito/ebpf-test
* https://blog.px.dev/ebpf-http-tracing/
* https://polycube-network.readthedocs.io/en/latest/quickstart.html
* https://stackoverflow.com/questions/53874223/ebpf-program-to-forward-http-requests-to-different-port
* https://stackoverflow.com/questions/2775177/whats-the-bpf-for-http

PRs with implementation are wellcome ;)

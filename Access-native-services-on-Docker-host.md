# Access native services on Docker host via host.docker.internal

[article link](https://megamorf.gitlab.io/2020/09/19/access-native-services-on-docker-host-via-host-docker-internal/)

[Explore networking features](https://docs.docker.com/desktop/networking/)

Docker containers can access local services running on the host by connecting to host.docker.internal.

The host has a changing IP address (or none if you have no network access). We recommend that you connect to the special DNS name `host.docker.internal` which resolves to the internal IP address used by the host. This is for development purpose and does not work in a production environment outside of Docker Desktop.

You can also reach the gateway using `gateway.docker.internal`.

Note:

* it only works on Docker for Windows/Mac by default
* on Linux it’s useless for now but could be available starting from 20.03
* it’s Docker specific so it doesn't exist in CRI-O or ContainerD with Kubernetes

* The Docker docs say:

> The host has a changing IP address (or none if you have no network access). We recommend that you connect to the special DNS name host.docker.internal which resolves to the internal IP address used by the host. This is for development purpose and will not work in a production environment outside of Docker Desktop for Windows / Mac.  
Source: https://docs.docker.com/docker-for-windows/networking/#use-cases-and-workarounds

## Use Case

What does this allow you to do exactly?

You could spin up a database or perhaps a development version of something like Consul and want to access it from inside the container when using Docker for Mac/Windows. Using host.docker.internal will resolve the correct host IP every time, which will be handy when moving between networks etc (as you can’t use localhost as the docker engine is running in a VM).

## Implementation

In 20.03 ([moby/moby#40007](https://github.com/moby/moby/pull/40007)) added support for a magic string `host-gateway` that can be passed to ExtraHosts (–add-host) to reliably pass the Docker host IP to your containers.

Adding `--add-host=host.docker.internal:host-gateway` to `docker run` adds the host.docker.internal DNS entry in /etc/hosts and maps it to host-gateway-ip.

```bash
docker run -it --add-host=host.docker.internal:host-gateway alpine cat /etc/hosts
```

Or for docker-compose:

```bash
# docker-compose.yml
  my_app:
    image: ...
    extra_hosts:
      - "host.docker.internal:host-gateway"
```

This is also available as daemon flag called host-gateway-ip which defaults to the default bridge IP.

To have an identical behaviour across all Docker versions (Windows, Linux, Mac):

* add `"dns-resolve-docker-host": false,` in the Docker daemon config
* add `--add-host host.docker.internal=host-gateway` to your container

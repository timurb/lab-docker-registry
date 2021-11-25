# Docker registry

For running local labs you might want to do quite many image pulls and likely hit the limits imposed by Docker Inc.

For that sake you can run local Docker registry and do all pulls from there.

## Configuration

Open your Docker Desktop UI. Go to the "Docker Engine" tab and paste the following to your Docker Engine JSON config:
```
  "registry-mirrors": ["http://localhost:5000"]
```

## Usage

```
docker-compose up -d
docker pull alpine
docker-compose logs | grep alpine
```

If you see the loglines containing "alpine" from now on all pulls to the central Docker registry will go through local registry and use locally cached version first.

If the registry is not up it is simply ignored and the requests go to the central Docker registry.

**Note:** Docker engines running inside VirtualBox/Vagrant are still going to the central Docker registry. You might need to do additional configurations for them to make them use the proxy

## Limitations
### Pushes
No pushes supported (upstream limitation). To push to your registry temporarily comment out env var `REGISTRY_PROXY_REMOTEURL` in your `docker-compose.yml` and do all the pushes you need. When you are done put the env var back. Don't forget to restart registry on every change.

### Maintenance
No cache maintenance is done. Some garbage collection procedures are described at: https://docs.docker.com/registry/garbage-collection/

For now if you run out of space use the following command to clear the registry:
```
docker-compose down --volumes
```

## License and author
* License:: MIT
* Author:: Timur Batyrshin <timurb@hey.com>

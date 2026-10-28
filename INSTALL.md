# Installation / Portainer

**Experimental and unofficial.** Requires Linux AMD64, Docker, and an existing Nginx Proxy Manager **2.16.0** deployment.

## 1. Pull the image

Once its GHCR package has been uploaded and made public:

```bash
docker pull ghcr.io/hibert23/nginx-proxy-manager-hibertfix:2.16.0-hibertfix
```

## 2. Identify your existing container and preserve a rollback

```bash
docker ps --format '{{.Names}}  {{.Image}}' | grep -Ei 'proxy.manager|proxymanager'
```

Substitute the actual container name below:

```bash
NPM_CONTAINER=nginxproxymanager
docker exec "$NPM_CONTAINER" sh -c 'echo "NPM: $NPM_BUILD_VERSION"; nginx -v'
docker inspect "$NPM_CONTAINER" --format '{{range .Mounts}}{{println .Source " -> " .Destination}}{{end}}'
docker inspect "$NPM_CONTAINER" --format '{{index .Config.Labels "com.docker.compose.project"}}'
docker image tag "$(docker inspect -f '{{.Image}}' "$NPM_CONTAINER")" local/nginx-proxy-manager:pre-hibertfix-rollback
```

**Before redeploying:** Back up the current Docker Compose / Portainer Stack definition, `/data`, `/etc/letsencrypt`, and any external database. Preserve the **original** volume, network and port definitions. Do not remove volumes.

## 3. Update your existing Portainer Stack

Change **only** the NPM service's image line to:

```yaml
image: ghcr.io/hibert23/nginx-proxy-manager-hibertfix:2.16.0-hibertfix
```

Select **Update the stack**. The container will be recreated, causing a brief interruption.

## 4. Verify

Recheck the container name if Portainer has changed it. Then:

```bash
docker exec "$NPM_CONTAINER" nginx -t
docker exec "$NPM_CONTAINER" nginx -v
docker logs --tail 50 "$NPM_CONTAINER"
```

Expected OpenResty version: `1.31.8.1`.

Verify the management interface, configured proxy hosts, and HTTPS certificate validity. The interface should display `v2.16.0 hibertfix`.

## Rollback

If necessary, replace the image line in the Portainer Stack with:

```yaml
image: local/nginx-proxy-manager:pre-hibertfix-rollback
```

Recreate the service **without pulling the local rollback image**. If database migrations changed the data, restoring the corresponding pre-upgrade database backup may also be necessary.

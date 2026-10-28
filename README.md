# Nginx Proxy Manager – hibertfix

**Unofficial, experimental Docker image** based on **Nginx Proxy Manager 2.16.0** with **OpenResty 1.31.8.1**.

> [!WARNING]
> This is **not an official release** from the Nginx Proxy Manager maintainers and is **not a security audit**. It was built and tested privately. Back up your installation and keep your existing image available for rollback. Prefer an official patched release when one becomes available.

## Versions

| Component | Version |
| --- | --- |
| Nginx Proxy Manager | 2.16.0 |
| OpenResty / nginx | 1.31.8.1 |
| Certbot | 5.8.0 |
| Node.js | 22.23.4 |
| Target platform | Linux / AMD64 (x86_64) |

The NPM interface displays **v2.16.0 hibertfix**. Its backend reports version 2.16.0, preventing a false update notification for that same version.

## Container image

GHCR package (uploaded):

```text
ghcr.io/hibert23/nginx-proxy-manager-hibertfix:2.16.0-hibertfix
```

**Registry status:** The image has been uploaded to GHCR. Package visibility (public/private) still needs to be checked; anonymous pulls work only when the package is public.

Published manifest digest: `sha256:7b617c7ac8fbc492f3f1a1c7872b5df689ccf79b85bac5eaed2f2aa6a405eea4`.

Once the package is public, pull it with:

```bash
docker pull ghcr.io/hibert23/nginx-proxy-manager-hibertfix:2.16.0-hibertfix
```

See **[INSTALL.md](INSTALL.md)** for backup, Portainer deployment, checks, and rollback.

## Provenance

- Upstream application: [NginxProxyManager/nginx-proxy-manager](https://github.com/NginxProxyManager/nginx-proxy-manager), tag `v2.16.0`, commit `a29eae9dc271ceabda51a6197bf1790644c99202`.
- Updated OpenResty base: [docker-nginx-full PR #41](https://github.com/NginxProxyManager/docker-nginx-full/pull/41), fork commit `3fccda26b97b41011a7cd43749bebb558910b76e`.
- The final application image is derived from the custom OpenResty base and includes a rebuilt frontend with a distinct `hibertfix` label.
- This repository documents and distributes the custom build; it is **not** a fork or mirror of the complete upstream source tree.

## Testing performed

Reported tests on a Debian 13 / x86_64 host using Docker and Portainer:

- Container starts; NPM backend and management interface work.
- `nginx -t` succeeds; `nginx -v` reports `openresty/1.31.8.1`.
- HTTPS reverse proxy returns HTTP 200 with a valid TLS certificate; HTTP and management interface return 200.
- All configured services were manually checked by the builder and reported working.

These checks do **not** establish compatibility with every NPM configuration or prove that all vulnerabilities are fixed.

## Credits and license

Nginx Proxy Manager is developed by [jc21 and contributors](https://github.com/NginxProxyManager/nginx-proxy-manager). The OpenResty update is based on the linked community contribution. Upstream copyright and license notices remain with their respective owners. The `hibertfix` name identifies this independently built variant, **not** an upstream endorsement.

## Updates

This is a version-pinned experimental image, not an automatically updated official image. Check upstream releases and the referenced PR for an official, maintained fix.

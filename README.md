# Ansible Seedbox

This is my personnal script for my seedbox installation and auto-update. It runs on CentOS 7.6.

## Current services

All services are served through [Traefik](https://traefik.io) acting as a reverse-proxy with auto TLS certificates renewal from [LetsEncrypt](https://letsencrypt.org/).

| Logo                                                                                                                                                                                                                    | Service    | Image                                                                                 |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------- | ------------------------------------------------------------------------------------- |
| <img src='https://raw.githubusercontent.com/Novik/ruTorrent/master/images/logo.png' width='40'>                                                                                                                         | ruTorrent  | [xataz/rtorrent-rutorrent](https://github.com/xataz/docker-rtorrent-rutorrent) :link: |
| <img src='https://raw.githubusercontent.com/Radarr/Radarr/develop/Logo/256.png' width='40'>                                                                                                                             | Radarr     | [linuxserver/radarr](https://github.com/linuxserver/docker-radarr) :link:             |
| <img src='https://raw.githubusercontent.com/Sonarr/Sonarr/develop/Logo/256.png' width='40'>                                                                                                                             | Sonarr     | [linuxserver/sonarr](https://github.com/linuxserver/docker-sonarr) :link:             |
| <img src='https://raw.githubusercontent.com/Jackett/Jackett/060972475f13ffe59fcc09c51ffe91a547a29029/src/Jackett.Common/Content/jacket_medium.png' width='40'>                                                          | Jackett    | [linuxserver/jackett](https://github.com/linuxserver/docker-jackett) :link:           |
| <img src='https://raw.githubusercontent.com/linuxserver/docker-templates/master/linuxserver.io/img/ombi.png' width='40'>                                                                                                | Ombi       | [linuxserver/ombi](https://github.com/linuxserver/docker-ombi) :link:                 |
| <img src='https://raw.githubusercontent.com/morpheus65535/bazarr/master/static/logo128.png' width='40'>                                                                                                                 | Bazarr     | [linuxserver/bazarr](https://github.com/linuxserver/docker-bazarr) :link:             |
| <img src='https://cdn6.aptoide.com/imgs/5/d/0/5d0ab62a64a947dc2060c8f7827847f5_icon.png' width='40'>                                                                                                                    | Plex       | [plexinc/pms-docker](https://github.com/plexinc/pms-docker) :link:                    |
| <img src='https://avatars3.githubusercontent.com/u/34385001' width='40'>                                                                                                                                                | Tautulli   | [tautulli/tautulli](https://github.com/tautulli/tautulli) :link:                      |
| <img src='https://avatars1.githubusercontent.com/u/22225832' width='40'>                                                                                                                                                | Portainer  | [portainer/portainer](https://github.com/portainer/portainer) :link:                  |
| <img src='https://avatars0.githubusercontent.com/u/181815?s=400&v=4' width='40'>                                                                                                                                        | H5ai       | [bixidock/h5ai](https://github.com/PixiBixi/dockerfiles/tree/master/h5ai) :link:      |
| <img src='https://camo.githubusercontent.com/7edd4ae7ae04b30fd707f9f9713e9778040b39ad/68747470733a2f2f30783132622e636f6d2f7761746368746f7765722d6c6f676f2e706e67' width='40'>                                           | Watchtower | [containrrr/watchtower](https://github.com/containrrr/watchtower) :link:              |
| <img src='https://docs.traefik.io/img/traefik.logo.png' width='40'>                                                                                                                                                     | Traefik    | [traefik](https://github.com/containous/traefik) :link:                               |
| <img src='https://raw.githubusercontent.com/thelounge/thelounge.github.io/master/assets/logos/logo/TL_Grey%26Yellow_Vertical_logotype_Transparent_Bg/TL_Grey%26Yellow_Vertical_logotype_Transparent_Bg.png' width='40'> | TheLounge  | [thelounge/thelounge](https://github.com/thelounge/thelounge-docker) :link:           |

## Logs & Metrics

Logs and metrics are collected using the [Datadog Agent](https://docs.datadoghq.com/agent/docker/) installed on the host. You will need an API key.

## Updates

Container auto-updates are managed using [Watchtower](https://github.com/containrrr/watchtower).

## Requirements

- You would need to define some variable according to your own hosts in `host_vars`.
- For hardware acceleration with Plex, you need a Plex Pass and to have a GPU/iGPU with the required driver. Some providers disable iGPU on purpose, you can re-enable it [like so](https://github.com/desimaniac/docs/blob/master/enable_igpu_on_hetzner.md). If you don't want to use hardware acceleration, remove the `device` attribute of the container in `roles/containers/tasks/plex.yml`.
- Wildcard-domains certificates need a [DNS Challenge](https://docs.traefik.io/configuration/acme/#wildcard-domains). You can configure your own in `roles/p2p/templates/traefik.toml.j2`
- The RAID0 configuration and partitions definition is not included in this script.
- This is also a good idea to define separate partitions for you media files and your `$DOCKER_HOME` (usually `/var/lib/docker`).
- It is a good idea to setup `fail2ban` for blocking suspicious SSH connections.
- The passlib (or python3-passlib or python-passlib) OS package must be installed on the system where Ansible runs, to prevent errors like "crypt.crypt does not support 'bcrypt' algorithm."

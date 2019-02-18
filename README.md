# Ansible Seedbox

This is my personnal script for my seedbox installation and auto-update.

## Current services

All services are served through [Traefik](https://traefik.io) acting as a reverse-proxy with auto TLS certificates renewal from [LetsEncrypt](https://letsencrypt.org/).

| Logo                                                                                                                                                           | Service   | Link                                                                             | Reverse-proxy      |
| -------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------- | -------------------------------------------------------------------------------- | ------------------ |
| <img src='https://raw.githubusercontent.com/Novik/ruTorrent/master/images/logo.png' width='40'>                                                                | ruTorrent | [link](https://github.com/Novik/ruTorrent) :link:                                | :white_check_mark: |
| <img src='https://raw.githubusercontent.com/Radarr/Radarr/develop/Logo/256.png' width='40'>                                                                    | Radarr    | [link](https://github.com/Radarr/Radarr) :link:                                  | :white_check_mark: |
| <img src='https://raw.githubusercontent.com/Sonarr/Sonarr/develop/Logo/256.png' width='40'>                                                                    | Sonarr    | [link](https://github.com/Sonarr/Sonarr) :link:                                  | :white_check_mark: |
| <img src='https://raw.githubusercontent.com/Jackett/Jackett/060972475f13ffe59fcc09c51ffe91a547a29029/src/Jackett.Common/Content/jacket_medium.png' width='40'> | Jackett   | [link](https://github.com/Jackett/Jackett) :link:                                | :white_check_mark: |
| <img src='https://raw.githubusercontent.com/linuxserver/docker-templates/master/linuxserver.io/img/ombi.png' width='40'>                                       | Ombi      | [link](https://github.com/tidusjar/Ombi) :link:                                  | :white_check_mark: |
| <img src='https://raw.githubusercontent.com/morpheus65535/bazarr/master/static/logo128.png' width='40'>                                                        | Bazarr    | [link](https://github.com/morpheus65535/bazarr) :link:                           | :white_check_mark: |
| <img src='https://cdn6.aptoide.com/imgs/5/d/0/5d0ab62a64a947dc2060c8f7827847f5_icon.png' width='40'>                                                           | Plex      | [link](https://www.plex.tv/apps-devices/#modal-devices-plex-media-server) :link: | :white_check_mark: |
| <img src='https://avatars3.githubusercontent.com/u/34385001' width='40'>                                                                                       | Tautulli  | [link](https://github.com/Tautulli/Tautulli) :link:                              | :white_check_mark: |
| <img src='https://avatars1.githubusercontent.com/u/22225832' width='40'>                                                                                       | Portainer | [link](https://portainer.io/) :link:                                             | :white_check_mark: |

## Logs

Logs are monitored using the [Datadog Agent](https://docs.datadoghq.com/agent/docker/). You will need an API key.

## Updates

Container updates are managed using [Watchtower](https://github.com/v2tec/watchtower)

## Requirements

- You would need to define some variable according to your own hosts in `host_vars`.

- For hardware acceleration with Plex, you need a Plex Pass and to have a GPU/iGPU with the required driver. Some providers disable iGPU on purpose, you can re-enable it [like so](https://github.com/desimaniac/docs/blob/master/enable_igpu_on_hetzner.md). If you don't want to use hardware acceleration, remove the `device` attribute of the container in `roles/containers/tasks/plex.yml`.

- Wildcard-domains needs a DNS Challenge. You can configure your own in `roles/p2p/templates/traefik.toml.j2`

- The RAID0 configuration and partitions definition is not included in this script.

- This is also a good idea to define separate partitions for you media files and your `$DOCKER_HOME` (usually `/var/lib/docker`).

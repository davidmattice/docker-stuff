# Docker Setup Stuff for my Lab

This is an attempt to document the setup I have used for my lab docker setup

## Host based mounts

Create these mounts on the *docker host*
```
mkdir -p /mnt/ds1019/containers/portainer_data
mkdir -p /mnt/ds1019/containers/nginx/conf.d
mkdir -p /mnt/ds1019/containers/gitlab/config
mkdir -p /mnt/ds1019/containers/gitlab/logs
mkdir -p /mnt/ds1019/containers/gitlab/data
mkdir -p /mnt/ds1019/containers/gitlab-runner/config
```

## Host DNS resolution

Update the order of resolution to use the local DNS server first
```
resolvectl dns <iface> <local dns host ip> <router>
```

## SSH for Gitlab

Client side configuration for gitlab access on a different port
```
@cat ~/.ssh/config
Host gitlab.home.local
    user git
    port 8022
```

Server side update required to *docker host* to uncomment this line
```
grep PubkeyAuthentication /etc/ssh/sshd_config
PubkeyAuthentication yes
```

## Portainer Setup

## Nginx Setup

## Gitlab Setup

## Gitlab Runner Setup

Run the following commands on the *docker host*
```
mkdir /mnt/ds1019/containers/gitlab-runner/config/certs
cp /mnt/ds1019/containers/gitlab/config/ssl/docker1.home.local.crt /mnt/ds1019/containers/gitlab-runner/config/certs
docker run --rm -it -v /srv/gitlab-runner/config:/etc/gitlab-runner gitlab/gitlab-runner register --tls-ca-file /etc/gitlab-runner/certs/docker1.home.local.crt
```

Update the runner configuration
```
Update config.toml
add:  "clone_url = "https://docker1.home.local:8443/""
updated:  image = "docker:stable"
updated:  volumes = ["/cache","/var/run/docker.sock:/var/run/docker.sock"]
```

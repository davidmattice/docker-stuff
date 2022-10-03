

## SSH

Client side configuration for gitlab access on a different port
```
@cat ~/.ssh/config
Host gitlab.home.local
    user git
    port 8022
```

Server side update required to *ubuntu 20.04* to uncomment this line
```
grep PubkeyAuthentication /etc/ssh/sshd_config
PubkeyAuthentication yes
```
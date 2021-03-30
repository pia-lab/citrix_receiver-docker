# IMPORTANT

This repo and the container provided via Docker hub (`https://hub.docker.com/r/eddyhub/citrix_receiver-docker`) IS NO LONGER MAINTAINED by me. Please use the new repo `https://github.com/eddyhub/citrix_workspace_app-container` and the the container `https://hub.docker.com/r/eddyhub/citrix_workspace_app-container`

# Docker container for the Citrix Receiver Client

This is a simple container to run the Citrix Receiver using the firefox browser.

## How to build this docker image
```
docker build -t citrix_receiver .
```

## Start the container
```
docker run -d -v /etc/timezone:/etc/timezone:ro -v /etc/localtime:/etc/localtime --name citrix_receiver citrix_receiver
```

## Connect to the container via ssh and X-Forward
Please set the WEB_URL_TO_LOGIN to your specific login url
```
ssh -f -X receiver@$(docker inspect --format '{{ .NetworkSettings.IPAddress }}' citrix_receiver) -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no /usr/bin/firefox <WEB_URL_TO_LOGIN> > /dev/null 2>&1
```


# Sources
[Ubuntu wiki](https://wiki.ubuntuusers.de/Citrix_Receiver_13/)

# Desktop launcher

To add a desktop launcher, execute on the host system:

```
$ cat | sudo tee /usr/local/bin/citrix-launcher <<EOF
#!/bin/bash
/usr/bin/docker restart citrix_receiver
/usr/bin/ssh -f -X receiver@$(docker inspect --format "{{ .NetworkSettings.IPAddress }}" citrix_receiver) -o UserKnownHostsFile=/dev/null -o StrictHostKeyChecking=no /usr/bin/firefox <MY_CITRIX_URL>
EOF
$ sudo chmod +x /usr/local/bin/citrix-launcher
$ cat | sudo tee /usr/share/applications/citrix.desktop <<EOF
[Desktop Entry]
Encoding=UTF-8
Name=Citrix CL
Exec=/usr/local/bin/citrix-launcher
Icon=/usr/share/icons/default/citrix-receiver-icon.png
Type=Application
Categories=Development
EOF
$ sudo wget https://www.freeiconspng.com/uploads/citrix-icon-3.png -O https://www.freeiconspng.com/uploads/citrix-icon-3.png
```

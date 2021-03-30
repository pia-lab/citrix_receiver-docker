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

# Certificats

Donc pour info, nous avons du ajouter "à la main" les certificats CRT de Thawte dans Firefox, ainsi que dans :
cp Downloads/ThawteTLSRSACAG1.crt thawte_Primary_Root_CA.crt /opt/Citrix/ICAClient/keystore/intcerts/ && /opt/Citrix/ICAClient/util/ctx_rehash

Par ailleurs et au besoin, voici la manière dont nous utilisons Docker :
https://github.com/pia-lab/citrix_receiver-docker

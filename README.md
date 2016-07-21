## Overview
This Docker image contains a deployed Shibboleth IdP 2.4.4 running on Java Runtime 1.7 update 79 and Jetty 9.2.10 running on the latest CentOS 7 base. 
## Running
$ docker build --tag="tableau/shibboleth-idp-2.4.x" .

$ docker run -dP --name="idp-tableou-host" --net host -v ~/docker/shib-config-2.4.x:/external-mount tableau/shibboleth-idp-2.4.x

$ docker exec -it idp-tableou-host   reset-idp.sh

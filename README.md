## Overview
This Docker image contains a deployed Shibboleth IdP 2.4.4 running on Java Runtime 1.7 update 79 and Jetty 9.2.10 running on the latest CentOS 7 base. 
## Running
$ docker build --tag="tableau/shibboleth-idp-2.4.x" .

$ docker run -dP --name="idp-tableou-host" --net host -v ~/docker/shib-config-2.4.x:/external-mount tableau/shibboleth-idp-2.4.x

$ docker exec -it idp-tableou-host   reset-idp.sh




## Configuring IDP:

$ cd /home/ec2-user/docker/shib-config-2.4.x

$ cd conf/

$ grep provider relying-party.xml   | grep -oE "provider=\"[^\"]+\""   | tail -n1  | sed 's/provider="https[:]\/\///g'   | sed 's/\/idp\/shibboleth"//g'

####### idp-eddy.loan-data.com

$ sudo sed   -i 's/idp-eddy.loan-data.com/tableau-idp.tdev2.com/g'  relying-party.xml

$ grep "RelyingParty id"  relying-party.xml

####### <rp:RelyingParty id="https://sso.online.tableau.com/public/sp/metadata?alias=cea1c3e9-3e1d-4311-92f3-28d5074e2c04"

$ sudo sed   -i 's/cea1c3e9-3e1d-4311-92f3-28d5074e2c04/f84e9711-a791-42c4-8b0d-ff9725d7b720/g'  relying-party.xml

$ sudo sed   -i 's/cea1c3e9-3e1d-4311-92f3-28d5074e2c04/f84e9711-a791-42c4-8b0d-ff9725d7b720/g'  relying-party.xml

$ cd ../

$ sudo mkdir metadata

$ sudo wget https://sso.online.tableau.com/public/sp/metadata?alias=f84e9711-a791-42c4-8b0d-ff9725d7b720  -O metadata/tableau-metadata.xml

$ docker exec -it idp-tableou-host   reset-idp.sh

$ docker exec -it idp-tableou-host   import.sh

$ docker restart  idp-tableou-host

####### Checking that container is running

$ docker ps -a

####### IDP metadata here:  https://tableau-idp.tdev2.com/idp/shibboleth

$ wget https://tableau-idp.tdev2.com/idp/shibboleth  -O  idp_metadata.xml  --no-check-certificate


## Configuring Tableau
- Select Settings and then Authentication. 
- Below the configured SAML connection, select Edit Connection.... 
- Under  Import metadata file into Tableau Online Select Browse IdP metadata file and then click Apply. Select Test Connection. 
- Under Embedding options, select Authenticate using an inline frame.
- Under Default authentication type for embedded views, select SAML.

# containers
Various examples showcase the use of **podman** binary to start containers/pods on your own Linux workstations/servers.<br>
The goal is to review the flexibility of containers with standalone deployments with the **podman** binary.<br><br>
This information will assist with understanding containers within a **Kubernetes cluster** <br><br>
**Important Note:** These containers are the exact same containers deployed with **podman**, **docker**, **helm**, **kubectl**, **oc** , etc.<br>


## jasperreportserver
Quickly deploy the Jasper Report Server with podman.  This example showcases the use of podman's network/volumes with two (2) containers.
[jasperreportserver](jasperreportserver)
![image](https://github.com/anapartner-com/containers2/assets/51460618/7b2af32a-4a14-4eff-b932-f5650da3d571)
[jasperreportserver container](https://hub.docker.com/r/bitnami/jasperreports/tags)


## nessus
Quickly deploy the Nessus Vulnerability Server with podman.  This example requires a free activation code from [Tenable](https://www.tenable.com/products/nessus/nessus-essentials) to use this example. 
Please note that it may take 60-120 minutes for the full Nessus database to download to your pod.
[nessus](nessus)
![image](https://github.com/anapartner-com/containers2/assets/51460618/0dec958b-662a-4662-b6e9-d7156e48bd65)
[nessus container](https://hub.docker.com/r/tenable/nessus/tags)


## pgadmin-postgres
This example will deploy both PostGresDB and the third-party UI of PGAdmin within a single pod.  Please observe two (2) containers using the same network space.  Also included is a post-configuration step using a JSON integration configuration file added to the running container.   This example also uses LetsEncrypt Certs as parameters to the container via volume switches. 
[pgadmin](pgadmin)
![image](https://github.com/anapartner-com/containers2/assets/51460618/ec8af449-5236-41d9-969c-ab82998e5e54)
[pgadmin4 container](https://hub.docker.com/r/dpage/pgadmin4/tags)
[postgresdb container](https://hub.docker.com/_/postgres/tags)


## splunk
Quickly deploy Splunk using the provided free license parameter.  This example has a process to modify a configuration file after the fact in the running container.
[splunk](splunk)
![image](https://github.com/anapartner-com/containers2/assets/51460618/83ea975b-9194-4869-89bb-319b5df50501)
[splunk container](https://hub.docker.com/r/splunk/splunk/tags)


## View of running containers/pods
To debug, we recommend using  **--log-level=debug**    AND   **podman logs -f <container_name>**   to monitor any startup behavior.<br>
Use  **podman exec -u root -it <container_name> sh**    to view any internal container challenges, configurations, or ENV parameters.
![image](https://github.com/anapartner-com/containers2/assets/51460618/48b0ccac-bf69-4ddb-8032-22620988fe56)


## TLS notes
Additional notes on using TLS certs with containers' environment or volume parameters.  Use LetsEncrypt **cert.pem** and  **privkey.pem**  with the containers to avoid any challenge/enforcement with browsers and the HTTPS protocol.<br>
[letsencrypt](https://anapartner.com/2023/11/26/streamlining-with-letsencrypt-wildcard-certificates-and-automated-validation/)<br>
![image](https://github.com/anapartner-com/containers2/assets/51460618/989706b3-9c05-47bf-a5c9-18156947f42e)
<br>Blog entry of LetsEncrypt Certbot binary (without podman) [certbot](https://anapartner.com/2023/07/13/lets-encrypt-dns-challenge/)
<br>
#### Create your master chain pem file (with the correct public root CA cert)
curl -sOL https://letsencrypt.org/certs/isrgrootx1.pem<br>
cat cert.pem isrgrootx1.pem chain.pem > combined_chain_with_cert.pem<br>
<br>

#### Create a 'RSA' type JKS and P12 Keystore using LetsEncrypt certs.
openssl pkcs12 -export -inkey privkey.pem  -in  cert.pem -name tomcat -out keyStore.p12 -password pass:changeit<br>

keytool -v -importkeystore -srckeystore  keyStore.p12 -srcstoretype PKCS12 -srcstorepass changeit -destkeystore keyStore.jks -deststoretype JKS -deststorepass changeit -keyalg RSA -noprompt<br>
<br>


### mitm
Blog entry on use of podman to deploy a Man-In-The-Middle proxy.
[mitm](https://anapartner.com/2023/07/13/secure-application-introspection/)
<br>
<br>

## qBittorrent and Kiwix 
Using two (2) containers, we can rapidly download large (5-100 GB) data files to our local workstation to service a local edition of Wikipedia and other sites in ZIM extension.<br>
The English edition of Wikipedia with all images and content is over 102 GB, and has been observed to take 4-8 hours to download. <br>
With the use of qBittorrent, we can get this duraction down to less than 15 minutes. <br>
This shell script will select only those files that have the string "_en_all_maxi" within the file name.<br>
[qBittorrent](qbittorrent) [qBittorrent container](https://github.com/linuxserver/docker-qbittorrent)
![image](https://github.com/anapartner-com/containers2/assets/51460618/94a519a7-e3a1-45e9-93ae-2d30b30f7bb6)
<br>
[Kiwix](kiwix-serve) [Kiwix container](https://github.com/kiwix/kiwix-tools/tree/main/docker)
![image](https://github.com/anapartner-com/containers2/assets/51460618/fc40dc5d-9079-4236-8242-324c19cff510)
<br>
[Full Kiwix Library](https://wiki.kiwix.org/wiki/Content_in_all_languages)
<br>
[Online Kiwix Library](https://library.kiwix.org/#lang=eng)


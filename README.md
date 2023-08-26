### Deploying a Local Secure OpenVAS
---

- **Generate SSL/TLS Certificates**
```shell
sudo apt install libnss3-tools  # install network security service library
Arch=amd64; OS=linux; Version=1.4.4 # declare mkcert variables

# download and install mkcert utility
sudo wget https://github.com/FiloSottile/mkcert/releases/download/v${Version}/mkcert-v${Version}-${OS}-${Arch} -O /usr/local/bin/mkcert
sudo chmod 775 /usr/local/bin/mkcert

# install certificate authority and generate certificate
mkcert -install # install CA
mkcert gvm localhost 127.0.0.1 ::1  # generate cert and key
mv gvm+3-key.pem gvm-key.pem  # rename the key
mv gvm+3.pem gvm.pem  # rename the cert

```

- **Clone and Deploy OpenVAS**
```shell
PASSWORD="$INSERT_PASSWORD_HERE";PROJECT="$INSERT_PROJECTNAME_HERE";
git clone https://github.com/knoxknot/local-secure-openvas; cd local-secure-openvas
docker compose -f docker-compose.yml -p $PROJECT pull # pull all the images
docker compose -f docker-compose.yml -p $PROJECT up -d # start all container
docker compose -f docker-compose.yml -p $PROJECT \
  exec -u gvmd gvmd gvmd --user=admin --new-password=$PASSWORD # change default password
docker compose -f docker-compose.yml -p $PROJECT logs -f # view logs
```

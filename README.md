# Exercício 1

Foram usados os seguintes comandos, em ordem:
```
docker run ubuntu:latest
docker run -it ubuntu /bin/bash
apt-get -y update
apt-get -y install curl
```

Para mostrar a versão do curl: `curl -V`
```
curl 7.81.0 (x86_64-pc-linux-gnu) libcurl/7.81.0 OpenSSL/3.0.2 zlib/1.2.11 brotli/1.0.9 zstd/1.4.8 libidn2/2.3.2 libpsl/0.21.0 (+libidn2/2.3.2) libssh/0.9.6/openssl/zlib nghttp2/1.43.0 librtmp/2.3 OpenLDAP/2.5.16
Release-Date: 2022-01-05
Protocols: dict file ftp ftps gopher gophers http https imap imaps ldap ldaps mqtt pop3 pop3s rtmp rtsp scp sftp smb smbs smtp smtps telnet tftp 
Features: alt-svc AsynchDNS brotli GSS-API HSTS HTTP2 HTTPS-proxy IDN IPv6 Kerberos Largefile libz NTLM NTLM_WB PSL SPNEGO SSL TLS-SRP UnixSockets zstd
```

# Exercício 2
- Cria-se um arquivo app.js com o exemplo básico do Express
- Cria-se um Dockerfile da seguinte maneira:

```
FROM node:alpine

COPY . /app

WORKDIR /app

RUN npm install

EXPOSE 8080

CMD ["node", "/app/app.js"]
```

- Builda a imagem com o comando `docker build -t mjrsf/node-devops .`
- Roda com o comando docker `run -d -p 8080:3000 mjrsf/node-devops`
- Verifica se o container está rodando com o comando `docker ps`
- Abre o navegador e, entrando no localhost (`localhost:8080`), verificamos também seu funcionamento

# Exercício 3

- Cria-se o docker-compose.yaml

```
volumes:
    app-vol:

networks:
    app-net:

services:
    app-node:
        depends-on:
            - postgres-app
            - nginx-app
        build: .
        command: node /app/app.js
        ports:
            - 3000:3000
        networks:
            - app-net
            
    nginx-app:
        image: nginx:lastest
        ports:
            - 80:80
        networks:
            - app-net
            
    postgres-app:
        image: postgres:lastest
        ports:
            - 5432:5432
        volumes: 
            - app-vol:/var/lib/postgresql/data
        networks:
            - app-net
```
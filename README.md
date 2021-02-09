# webdav

## Reference

http://nginx.org/en/docs/http/ngx_http_dav_module.html

https://github.com/arut/nginx-dav-ext-module

## Features

Compilación de código fuente nginx + http_dav_module + instalación nginx-dav-ext-module, tamaño de imagen pequeño 



Soporte para -e USERNAME xxx -e PASSWORD xxxconfigurar un inicio de sesión de usuario único

Soporte -v /your/htpasswd:/opt/nginx/conf/htpasswd:rod inicio de sesión multiusuario

El inicio de sesión multiusuario tiene mayor prioridad 

## Github

https://github.com/duxlong/webdav

## Docker hub

https://hub.docker.com/r/duxlong/webdav

## Usage

docker pull
```
docker pull duxlong/webdav
```

docker run 根据自己情况修改-单用户
```
docker run -d \
    -v /srv/dev-disk-by-label-2T/download:/data/download \
    -v /srv/dev-disk-by-label-3T/photo:/data/photo \
    -v /srv/dev-disk-by-label-3T/video:/data/video \
    -v /srv/dev-disk-by-label-3T/zoo:/data/zoo \
    -e USERNAME=xxx \
    -e PASSWORD=xxx \
    -p 8001:80 \
    --restart=unless-stopped \
    --name=webdav \
    duxlong/webdav
```
Docker ejecutar modificar según su propia situación: usuario único 
```
docker run -d \
    -v /srv/dev-disk-by-label-2T/download:/data/download \
    -v /srv/dev-disk-by-label-3T/photo:/data/photo \
    -v /srv/dev-disk-by-label-3T/video:/data/video \
    -v /srv/dev-disk-by-label-3T/zoo:/data/zoo \
    -v /docker/webdav/htpasswd:/opt/nginx/conf/htpasswd:ro \
    -p 8001:80 \
    --restart=unless-stopped \
    --name=webdav \
    duxlong/webdav
```
Docker ejecutar modificado según su propia situación-multiusuario 




- Soporte multiusuario; embarcación previa a la ejecución, los sitios en línea deben generar y configurar htpasswdarchivos (el algoritmo de cifrado predeterminado Md5)
- La necesidad de compartir archivos en el montaje. /datadirectorio de
- Información de usuario montada en /opt/nginx/conf/htpassw del directorio


```
version: "2"
services:
  webdav:
    container_name: webdav
    image: duxlong/webdav
    network_mode: bridge
    restart: unless-stopped
    volumes:
      # 挂载共享文件夹
      - /srv/dev-disk-by-label-2T/download:/data/download
      - /srv/dev-disk-by-label-3T/photo:/data/photo
      - /srv/dev-disk-by-label-3T/video:/data/video
      - /srv/dev-disk-by-label-3T/zoo:/data/zoo
      # 挂载用户名和密码
      - /docker/webdav/htpasswd:/opt/nginx/conf/htpasswd:ro
    ports:
      - 8001:80
```

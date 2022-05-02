### Deploy new docker image (update Nginx version)
1. Create docker image locally
```
docker build --build-arg ENABLED_MODULES="ndk lua" -t nginx-lua .
```
2. Tag and deploy to docker-hub
```
docker tag "nginx-lua" "tufin/nginx-lua:1.21.6"
docker push tufin/nginx-lua:1.21.6
```

See: https://github.com/nginxinc/docker-nginx/tree/master/modules

### Run nginx with lua module
```
docker run --name tufin-nginx-lua -v $PWD/nginx_js.conf:/etc/nginx/nginx.conf -p 8085:8085 -it --rm tufin/nginx-lua:1.21.6
```
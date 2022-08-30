# Nginx Dockerhub

## Use the Image

```sh
docker run \
    --name some-nginx \
    -v /some/content:/usr/share/nginx/html:ro \
    -d \
```

Or use a dockerfile 

```sh
FROM nginx
COPY static-html-directory /usr/share/nginx/html
```

place this file in the same directory as content

```sh
docker build -t some-content-nginx .

docker run --name some-nginx -d some-content-nginx 
```

## Exposing external port

```sh
docker run --name some-nginx -d -p 8080:80 some-content-nginx
```

http://localhost:8080

## Complex Config

```
docker run \
    --name my-custom-nginx-container \
    -v /host/path/nginx.conf:/etc/nginx/nginx.conf:ro 
    -d nginx
```

Using a Dockerfile

```Dockerfile
FROM nginx
COPY nginx.conf /etc/nginx/nginx.conf 
```

if add custom CMD in dockerfile, be sure to include ```-g daemon off;``` in the CMD in order for nginx to stay in the foreground, so that docker can track the process properly.

```
docker build -t custom-nginx .
docker run --name my-nginx-container -d custom-nginx
```

### Env Variables in config

```Dockerfile
web: 
    image: nginx
    volumes:
    - ./templates:/etc/nginx/templates
    ports:
    - "8080:80"
    environment:
    - NGINX_HOST=foobar.com
    - NGINX_PORT=80
```

this function reads template files in ```etc/nginx/templates/*.template``` and outputs the result of executing envsubst to ```/etc/nginx/conf.d```.

so if you place ```templates/default.conf.template``` file which contains variable reference like this

```
listen ${NGINX_PORT};
```

outputs to /etc/nginx/conf.d/default.conf the port 80.

## Running nginx as non-root user

requires modification of nginx configuration to use directories, writeable by UIG/GID pair

```
docker run -d -v $PWD/nginx.conf:/etc/nginx/nginx.conf nginx
```

where nginx.conf in hte current directory should have the following directives redefined

```yaml
pid /tmp/.pid;

# and in http context:
http {
    client_body_temp_path /tmp/client_temp;
    proxy_temp_path       /tmp/proxy_temp_path;
    fastcgi_temp_path     /tmp/fastcgi_temp;
    uwsgi_temp_path       /tmp/uwsgi_temp;
    scgi_temp_path        /tmp_scgi_temp;
}
```

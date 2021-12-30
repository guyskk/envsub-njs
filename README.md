# envsub-njs

Docker-friendly environment variables substitution for Nginx configuration.

Implemented via https://nginx.org/en/docs/njs/, use Javascript template literals.

## Example: Nginx config  

- Just write Javascript in `${  }` block, it's template literals.
- Use function: `Env(name:string, default?:string)`

```
location ^~ /api {
    proxy_set_header Host ${Env('NGINX_PROXY_HOST', '$http_host')};
    proxy_pass http://${Env('NGINX_UPSTREAM')};
}
```

## Example: Docker file  

```Dockerfile
FROM nginx:stable-alpine

RUN curl -L https://github.com/guyskk/envsub-njs/releases/download/v0.1.0/envsub-njs.sh -o /usr/local/bin/envsub-njs && chmod +x /usr/local/bin/envsub-njs

RUN echo '/usr/local/bin/envsub-njs /etc/nginx/nginx.conf' \
        > /docker-entrypoint.d/01-envsub-njs-nginx.sh && \
    chmod +x /docker-entrypoint.d/01-envsub-njs-nginx.sh

# When container starts, it will auto execute scripts in /docker-entrypoint.d
```

## How it works?

Source code is very short, take a look :)

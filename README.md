# nginx_sticky_module_ng
Fork of https://bitbucket.org/nginx-goodies/nginx-sticky-module-ng/, tag version `1.2.6` and hash `c78b7dd79d0d`.

## NGINX 1.24.x compatibility
Patches were developed and applied in sticky module to guarantee compatibility to NGINX 1.24.x branch.

## Compiling this module as Dynamic Module
Here are my references to build this module as Dynamic Module:
https://www.nginx.com/blog/compiling-dynamic-modules-nginx-plus/
https://www.nginx.com/resources/wiki/extending/new_config/
https://www.nginx.com/resources/wiki/extending/converting/

### Step-by-step:
`Step 1`: Verify and obtain the NGINX Open Source Release

    $ nginx -v
    nginx version: nginx/1.24.0
    $ wget https://nginx.org/download/nginx-1.24.0.tar.gz
    $ tar -xzf nginx-1.24.0.tar.gz

`Step 2`: Obtain the Sticky Module Sources

    $ git clone git@github.com:fabianofurtado/nginx_sticky_module_ng.git

`Step 3`: Compile the Sticky Dynamic Module

    $ cd nginx-1.24.0/
    $ ./configure --with-compat --add-dynamic-module=../nginx_sticky_module_ng
    $ make modules

`Step 4`: Copy the module library (.so file) to /etc/nginx/modules as root

    $ su -
    # mkdir -p /etc/nginx/modules/
    # cp objs/nginx_sticky_module.so /etc/nginx/modules/

`Step 5`: Configure nginx.conf file

    # #add the load_module directive in the top‑level (main) context of your nginx.conf configuration file (not within the http, stream or mail context):
        load_module modules/nginx_sticky_module.so;

`Step 6`: Add "sticky;" directive inside your upstream configuration

    upstream ups_sticky {
      sticky;
      server <ip1>:<port>;
      server <ip2>:<port>;
    }

`Step 7`: Reload NGINX

    # nginx -s reload

`Step 8`: Check out for the `route` cookie

    $ curl -v http://localhost/
    ...
    < Set-Cookie: route=...; Path=/
    ...

## Versioning
v0.0.2 - 2023-06-23 - nginx 1.24.x sticky DYNAMIC module compilation compatibility

v0.0.1 - 2023-06-18 - nginx 1.24.x sticky module compatibility
# nginx-brotli-modules
A shell script to build compliant Brotli modules with your current Nginx server version.

## How to build compliant Brolti modules with your current Nginx server version
- Download the file ``build-nginx-brotli-modules.sh`` to your computer/server where Nginx is installed
- Run this command from your computer/server:
  ```
  bash build-nginx-brotli-modules.sh
  ```
- After some time, you should have those two files created in your current folder (where you've executed the script):
  - ``ngx_http_brotli_filter_module.so``
  - ``ngx_http_brotli_static_module.so``
 
## How to get Brotli working with your Nginx server
- Move the two files above to ``/usr/lib/nginx/modules``
- Prepend those two lines at the top of ``/etc/nginx/nginx.conf``:
```
load_module modules/ngx_http_brotli_filter_module.so;
load_module modules/ngx_http_brotli_static_module.so;
```
- In your ``http`` block of ``/etc/nginx/nginx.conf``, insert those lines:
```
brotli on;
brotli_comp_level 6;
brotli_static on;
brotli_types application/atom+xml application/javascript application/json application/rss+xml
  application/vnd.ms-fontobject application/x-font-opentype application/x-font-truetype
  application/x-font-ttf application/x-javascript application/xhtml+xml application/xml
  font/eot font/opentype font/otf font/truetype image/svg+xml image/vnd.microsoft.icon
  image/x-icon image/x-win-bitmap text/css text/javascript text/plain text/xml;
```
- Then, test your Nginx configuration: ``sudo nginx -t``
- Finally, reload your Nginx configuration: ``sudo nginx -s reload``

## What this script actually does
- It checks if Nginx is installed on your system.
- It tries to install ``build-essential`` ``cmake`` ``git`` ``libpcre3-dev`` ``tar`` ``wget`` packages, if not installed yet.
- It clones the Google's Nginx Brotli source repo ``https://github.com/google/ngx_brotli``.
- It starts to build the modules based on the source repo cloned.
- It continues by downloading the Nginx source version of your Nginx server installed.
- It actually builds the compliant Nginx Brotli modules.
- Finally it moves the modules to the current active folder & deletes the working folders.

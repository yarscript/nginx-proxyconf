# nginx-proxyconf
Nginx proxy vhost conf

```
server {
        listen 80 default_server;
        listen [::]:80 default_server;

        root /var/www/example/build;

        # Add index.php to the list if you are using PHP
        index index.html index.htm index.nginx-debian.html;

        server_name admin.example.com;

        location / {
                # First attempt to serve request as file, then
                # as directory, then fall back to displaying a 404.
                try_files $uri $uri/ =404;
                proxy_cache_valid any 30m;
                proxy_cache_min_uses 3;
                proxy_cache_bypass $cookie_nocache $arg_nocache$arg_comment;
        }

}

# Virtual Host configuration for example.com
#
# You can move that to a different file under sites-available/ and symlink that
# to sites-enabled/ to enable it.

server {
        listen 80;
        listen [::]:80;

        server_name api.example.com;

        root /var/www/example.com;
        index index.html;

        location / {
                proxy_set_header X-Forwarded-Server $host;
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_pass http://localhost:4000;
        }
}

```

VHost Proxy with WS
===================

```
server {
        listen 80;
        listen [::]:80;

        server_name api.example.com;

        #root /var/www/example.com;
        #index index.html;

        location / {
#        root /path/to/myapp/public;
                proxy_set_header X-Forwarded-Server $host;
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_set_header X-Forwarded-Proto $scheme;
                proxy_set_header X-Real-IP $remote_addr;
                proxy_pass http://localhost:4000;
                proxy_pass_request_headers on;
                proxy_set_header Upgrade $http_upgrade;
                proxy_set_header Connection $http_connection;
                proxy_set_header Host $host;
        }
}
```

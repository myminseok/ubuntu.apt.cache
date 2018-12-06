
# get ubuntu pkg (for apt-cache)
```
split -b 100000000 ubuntu-apt-cache-big.tar.gz ubuntu-apt-cache-big.tar.gz.part

cat ubuntu-apt-cache-big.tar.gz.part* > ubuntu-apt-cache-big.tar.gz
```

# ubuntu-14.04.3-server-amd64.iso (601mb)
```
http://old-releases.ubuntu.com/releases/14.04.3/
wget http://old-releases.ubuntu.com/releases/14.04.3/ubuntu-14.04.3-server-amd64.iso
```

# install base pkg
```
cd /var/cache/
tar xf apt-cache-ubuntu-trusty.tar.gz
cd /var/cache/apt/archives
dpkg -i *.deb
apt-get install -f

apt list --installed
dpkg -l

```

# setup nginx with compile
```
tar xf nginx-1.15.7.tar.gz
cd nginx-1.15.7
./configure
make && make install

```

# setup nginx (apt)

```
apt-get install nginx

vi /etc/nginx/sites-available/default

server {
  listen 80 default_server;
  listen [::]:80 default_server ipv6only=on;

  root /var/www/pypi/packages;
  index index.html index.htm;

  location / {
    try_files $uri $uri/ =404;
    autoindex on;
  }
}



mkdir -p /etc/nginx/ssl
cd /etc/nginx/ssl


openssl genrsa -des3 -passout pass:x -out server.pass.key 2048
openssl rsa -passin pass:x -in server.pass.key -out server.key
rm server.pass.key 
 openssl req -new -key server.key -out server.csr
openssl x509 -req -sha256 -days 365 -in server.csr -signkey server.key -out server.crt


vi /etc/nginx/sites-available/default

server {
listen 80 default_server;
listen [::]:80 default_server ipv6only=on;

root /var/www/pypi/packages;

index index.html index.htm;
        listen 443 ssl;
        server_name pypi.domain.local;
        ssl_certificate /etc/nginx/ssl/server.crt;
        ssl_certificate_key /etc/nginx/ssl/server.key;


location / {
# First attempt to serve request as file, then
# as directory, then fall back to displaying a 404.
try_files $uri $uri/ =404;
# Uncomment to enable naxsi on this location
# include /etc/nginx/naxsi.rules
                autoindex on;
}

service nginx restart

```

# OS verson check

```
lsb_release -a
```

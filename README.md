
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

# 
```
apt-get-bind9.tar.gz
libirs141_1%3a9.10.3.dfsg.P4-8ubuntu1.15_amd64.deb
openjdk-8-jre_8u222-b10-1ubuntu1~16.04.1_amd64.deb
openjdk8-ubuntu
pip9-ubuntu-trusty.tar.gz
screen_4.3.1-2build1_amd64.deb
trusty-server-cloudimg-amd64-disk1.img
ubuntu-14.04.3-server-amd64.iso
ubuntu-16.04.4-server-amd64.iso
ubuntu-apt-cache-big.tar.gz
ubuntu-apt-cache-pypi-mirror.tar.gz

```
# wso2
https://medium.com/@kosalasananthana/how-to-install-wso2-identity-server-f98c9d8b1c81
```
wso2is-linux-installer-x64-5.6.0.deb
```

# pypi mirror
- https://aboutsimon.com/blog/2012/02/24/Create-a-local-PyPI-mirror.html
- https://www.digitalocean.com/community/tutorials/how-to-create-an-ssl-certificate-on-nginx-for-ubuntu-14-04
- https://devcenter.heroku.com/articles/ssl-certificate-self

```
cd /var/cache/
tar xf apt-cache-ubuntu-trusty.tar.gz
cd /var/cache/apt/archives
dpkg -i *.deb
apt-get install -f

apt list --installed
dpkg -l

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


##### mirror

https://github.com/wolever/pip2pi
===> NICE


pip9-ubuntu-trusty.tar.gz

pip --version
pip 1.5.4 from /usr/lib/python2.7/dist-packages (python 2.7)

pip install pip-9.0.1-py2.py3-none-any.whl

pip2.7 --version
pip 9.0.1 from /usr/local/lib/python2.7/dist-packages (python 2.7)


pip install pip2pi

pypi-dash-goldman.tar.gz


mkdir -p /var/www/pypi/

pip2tgz packages/ -r requirements.txt

dir2pi packages/

https://docs.cloudfoundry.org/devguide/deploy-apps/trusted-system-certificates.html

https://docs.cloudfoundry.org/adminguide/trusted-system-certificates.html


openssl s_client -connect api.system.pcfdemo.net:443 < /dev/null | sed -ne '/-BEGIN CERTIFICATE-/,/-END CERTIFICATE-/p' > public.crt


openssl s_client -connect 192.168.11.255:443 < /dev/null | sed -ne '/-BEGIN CERTIFICATE-/,/-END CERTIFICATE-/p' > pypi.crt

sudo keytool -import -alias api.cf.lab.local -keystore $JAVA_HOME/jre/lib/security/cacerts -file /tmp/public.crt 

https://superuser.com/questions/437330/how-do-you-add-a-certificate-authority-ca-to-ubuntu

/usr/local/share/ca-certificates/foo.crt

update-ca-certificates

openssl s_client -connect pypi.domain.local:443 -CApath /etc/ssl/certs

openssl s_client -connect pypi.domain.local:443  

##### python upgrade on ubuntu trusty
http://mbless.de/blog/2016/01/09/upgrade-to-python-2711-on-ubuntu-1404-lts.html

#####pip upgrade
ValueError: ('Expected version spec in', '--trusted-host pypi.domain.local', 'at', ' pypi.domain.local')

pip install --upgrade pip
```

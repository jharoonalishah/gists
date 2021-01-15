## Installation & Setup
### Windows
Download stable version
- `http://nginx.org/en/download.html`
- Unzip nginx-x.x.x.zip and then place the folder to "C" drive.
- Install OpenSSL and add path to environment variables. `http://slproweb.com/products/Win32OpenSSL.html`
- Generate certification to optional directory.
```adal
C:nginx-x.x.x> mkdir cert
C:nginx-x.x.x> cd cert
C:nginx-x.x.x\cert> openssl req -x509 -newkey rsa:4096 -nodes -out server.pem -keyout server.key -days 365

#Input corresponding values when prompted.

```
```
Edit C:/nginx-x.x.x/conf/nginx.conf as below and replace paths.
{path_to_the_certification_dir}
{path_to_the_repository}
```
```bash
...
     
  server {

      ssl_certificate      {path_to_the_certification_dir}/server.pem;
      ssl_certificate_key  {path_to_the_certification_dir}/server.key;
    
...

```
C:nginx-x.x.x> start nginx

### Extras
Hosts settings

```bash
# ../hosts file
127.0.0.1             www.example.com
```
### Mac
brew update
brew install nginx

```bash
mkdir {any_path}/cert
cd {any_path}/cert

# change example.com to your website
openssl req -x509 -newkey rsa:4096 -nodes -out example.com.pem -keyout example.com.key -days 365
```
/usr/local/etc/nginx/nginx.conf
```bash
# Add vhost folder. This folder contains configuration for vhost domains

...

include vhosts/*.conf;

...
```

#### vhosts

```
# create following folder
mkdir -p /usr/local/etc/nginx/vhosts

# vhosts files
touch /usr/local/etc/nginx/vhosts/example.com.conf

# Add following configuration
server {
  listen       10443 ssl;
  server_name  example.com.local;

  ssl_certificate      {cert path}/example.com.pem;
  ssl_certificate_key  {cert path}/example.com.key;

  ssl_session_cache    shared:SSL:1m;
  ssl_session_timeout  5m;

  ssl_ciphers  HIGH:!aNULL:!MD5;
  ssl_prefer_server_ciphers  on;

  proxy_no_cache 1;


  # -------------------------------
  # Location to website resources
  # -------------------------------
  
  location ~ /(css|img|js)/ {
      root {path to pointonline/web local repository}/public;
  }

  location / {
      root   /usr/local/etc/nginx/html/example.com.local;
  }
}
```
#### Nginx
```
# start
brew services start nginx

# status
brew services list

# stop
brew services stop nginx
```

#### Trouble Shooting
```
tail /usr/local/var/log/nginx/error.log
```

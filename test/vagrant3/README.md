## In this environment, we will set up Ghost blogging platform behind Nginx which will act as a reverse proxy
----

### First, Manual Install

#### Installing and setting up Ghost

General format for installing Ghost blogging platform requires
- `node.js` and
- `npm`
Some misc tools:
- `swapspace` (since our VM is only 512MB, the yarn processes crash -- https://github.com/TryGhost/Ghost-CLI/issues/192)
- `curl` 

Generally if you are doing this manually, you would follow following steps:

Install a non-priviledged user that will deal with ghost processes so that even if your ghost is compromised, your system won't be.

```shell
sudo addgroup ghost
sudo useradd -r -s /bin/false ghost
```

Update the packages

``` shell
sudo apt-get update
sudo apt-get -y install swapspace curl
```

Now we'll install nvm (node version manager) so that we dont have to
sudo for installing packages

``` shell
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.2/install.sh | bash
source ~/.bashrc
```
Lets install nodejs (v6.11.2 or you can select lts) and sqlite3 (since we are simply going to install ghost in `local` mode)

``` shell
nvm install v6.11.2
npm install -g sqlite3
```

Now setup directory for ghost and install ghost via ghost-cli

``` shell
sudo mkdir -p /var/www/ghost
chmod -R 775 /var/www/ghost
chown -R $USER:$USER /var/www/ghost
cd /var/www/ghost
npm install ghost-cli@latest
ghost install local --url http://localhost --ip=0.0.0.0 --port 2368 --db-sqlite3 --no-prompt
```

You should now be able to access the blog at [http://localhost:2368](http://localhost:2368)
Lets stop it until we have completed remaining setup


#### Installing and setting up Nginx

Nginx will basically port-forward all the connections on port 80 to port 2368
where Ghost server is listening for incoming connections

``` shell
sudo apt-get install nginx
```

Lets configure the nginx's sites-available configuration to do the port-forwarding
- Create a new file at `/etc/nginx/sites-available/ghost`
- Add following config
  ``` nginx
  server {
  listen 80;
  server_name localhost;
    location / {
      proxy_set_header   X-Real-IP $remote_addr;
      proxy_set_header   Host      $http_host;
      proxy_pass         http://127.0.0.1:2368;
    }
  }
  ```
  
- Next, symlink this to `sites-enabled`
  ```shell
  sudo ln -s /etc/nginx/sites-available/ghost /etc/nginx/sites-enabled/ghost
  ```
  
- Finally, restart nginx
  ```shell
  sudo service nginx restart
  ```
  

### Ansible based install
> check playbook.yml


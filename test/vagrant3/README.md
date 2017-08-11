# In this environment, we will set up Ghost blogging platform behing Nginx
which will act as a reverse proxy

## First, Manual Install

### Installing and setting up Ghost

General format for installing Ghost blogging platform requires
- `forever` -- to keep Ghost always running
- `node.js` and
- `npm`
Some misc tools:
- `wget` -- to download stuff
- `unzip` -- to unzip the downloaded stuff

Generally if you are doing this manually, you would follow following steps:

``` shell
sudo apt-get update
sudo apt-get install unzip wget
```

Now we'll install nvm (node version manager) so that we dont have to
sudo for installing packages

``` shell
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.2/install.sh | bash
```
Now either logout and login again or close and open another terminal to install nodejs
and sqlite3

``` shell
nvm install --lts
npm install -g sqlite3
```

Now setup directory for ghost and install ghost via ghost-cli

``` shell
sudo mkdir -p /var/www/ghost
chown -R $USER:$USER /var/www
cd /var/www/ghost
npm install ghost-cli@latest
ghost install local --db-sqlite3
```

You should now be able to access the blog at [http://localhost:2368](http://localhost:2368)
Lets stop it until we have completed remaining setup

``` shell
ghost stop
```

### Installing and setting up Nginx

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

### Keep Ghost Running
This will be the final step where we would use the `forever` node module
to keep the ghost server running *forever*

``` shell
npm install -g forever
```
Now, back as ghost user and start ghost using `forever`

``` shell
su - ghost
cd /var/www/ghost
forever start current/index.js
```
This will start ghost in production environment. To stop the ghost
you can run `forever stop index.js`


## Ansible based install

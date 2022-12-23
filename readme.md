# problem

Mattermost is great but currently does not offer arm support.
This repo documents how to get mattermost up and running using docker compose utilizing the multi-os builds of mattermost created by SmartHoneybee.
For that the Dockerfile from Mattermost is used but the release package is replaced with the arm version from SmartHoneybee.

# setup

1. clone the mattermost repo

```
git clone https://github.com/mattermost/mattermost-server.git
```

2. checkout the repository to the desired version

```
git checkout v7.5.2 # same as variable in .env
```

# same as variable in .env

3. edit the dockerfile

```
vim mattermost-server/build/Dockerfile
```

- add `wget` to apt installations
- replace `curl $MM_PACKAGE | tar -xvz` with `wget $MM_PACKAGE --output-document mattermost.tar.gz && tar -xzvf mattermost.tar.gz`

4. `cp .env.example .env`
5. docker compose up -d
6. docker compose logs

# remaining issues

Because marketplace extension are build for amd64 only, they can't be installed on this arm.
If you want to use them, you will have to compile them for arm.

# nginx

An example configuration for nginx is provided in the `nginx` directory.
On debian based systems certs can be set up the following way:

1. install nginx and certbot

```
sudo apt install nginx certbot python3-certbot-nginx
```

2. get certificate

```
sudo certbot --nginx certonly -d '<domain-name>'
```

3. validate nginx config and restart it

```
sudo nginx -t
sudo nginx -s reload
```

4. automatic cert renewal

```
crontab -e
```

replace contents with:

```
# certification renewal
@weekly sudo certbot renew --nginx
```

# sources

- [releases of mattermost for all architectures](https://github.com/SmartHoneybee/ubiquitous-memory/releases/)
- [official mattermost dockerfile](https://github.com/mattermost/mattermost-server/blob/master/build/Dockerfile)
- [official mattermost docker-compose file](https://github.com/mattermost/docker/blob/main/docker-compose.yml)
- [nginx websocket configuration](https://www.nginx.com/blog/websocket-nginx/)
- [mattermost database configuration string](https://docs.mattermost.com/configure/configuation-in-mattermost-database.html)

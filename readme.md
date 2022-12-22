1. clone the mattermost repo
git clone https://github.com/mattermost/mattermost-server.git

2. edit the dockerfile
vim mattermost-server/build/Dockerfile

- add wget to apt installations
- replace `curl $MM_PACKAGE | tar -xvz` with `wget $MM_PACKAGE --output-document mattermost.tar.gz && tar -xzvf mattermost.tar.gz`

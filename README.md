# Introduction

This repository is to manage my personal server.

# Prerequisites
1. Tools
```sh
sudo apt update && sudo apt upgrade -y
sudo apt install -y git docker.io docker-compose
```
2. Github SSH
```sh
mkdir -p ~/.ssh
cat >> ~/.ssh/config << EOF
Host github.com
    HostName github.com
    User git
    IdentityFile ~/.ssh/github
    AddKeysToAgent yes
EOF
chmod 600 ~/.ssh/config
```
```sh
cat >> ~/.ssh/github << EOF
{{private SSH key}}
EOF
chmod 700 ~/.ssh/github
```

# Clone
```sh
git clone git@github.com:tiagoramirez/server.git &&
cd server
```

# Setup .env and modify it
```sh
cp .env.example .env
```

# Download dependencies
1. n8n
```sh
cd dependencies/
mv n8n/docker-compose.override.yml docker-compose.override.yml
rm -r n8n
git clone git@github.com:tiagoramirez/n8n.git
mv docker-compose.override.yml n8n/docker-compose.override.yml
cd ..
```

# Create network
```sh
docker network create web
```

# Run server
```sh
docker-compose -f docker-compose.yml -f dependencies/n8n/docker-compose.yml -f dependencies/n8n/docker-compose.override.yml up -d
```
# Deploying Django app on VPS - Quick commands


set up ssh:
```
cd .ssh
ssh-keygen -t ed25519
cat <key_name>.pub  
ssh -i ~/.ssh/<key_name> root@<vps_address>
```

Make new folder:
```
mkdir <app_name>
```

Update OS and packages:
```
sudo apt update
sudo apt upgrade
```

Moving files from your computer to the VPS:
```
tar --exclude='node_modules' \
 --exclude='frontend/dist' \
 --exclude='.env' \
 -czf app.tar.gz .

scp -i ~/.ssh/<key_name> app.tar.gz root@<vps_address>:~/<app_name>/
```

Unpack tar file on VPS:
```
tar -xzf app.tar.gz
```

Evironment Variables:
```
cp .env.example .env.prod
nano .env.prod
```

Caddy
```
nano caddyfile
```

Install Docker Compose
```
sudo apt-get install -y docker-compose-v2
```

Docker Compose, rebuild and restart
```
docker compose -f docker-compose.prod.yml up -d --build
```

See running containers
```
docker ps
```

See Caddy logs inside docker prod
```
docker compose -f docker-compose.prod.yml logs caddy
```

Run migrations inside docker prod
```
docker compose -f docker-compose.prod.yml exec backend python manage.py migrate
```




Rsync for those that have linux/mac:
```
rsync -avz --exclude node_modules --exclude frontend/dist --exclude .env -e "ssh -i ~/.ssh/<key_name>" .  root@<vps_ip>:~/<folder_name>/
```





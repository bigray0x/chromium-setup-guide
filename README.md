# chromium setup guide for VPS

<img width="645" alt="Screenshot 2025-06-14 at 7 16 13 AM" src="https://github.com/user-attachments/assets/a6da050f-ea91-4ee3-8221-ccf51e335f92" />

### Chromium browser is a full capacity browser that runs on your virtual machine.

Why use chromium?

- you get 24h uptime.
- Reliability and Efficieny.
- Better for Depins (preserve pc battery life)
- You can run multiple chromiums on one VPS.


### Step 1 : Update machine and install docker :

```
sudo apt update -y && sudo apt upgrade -y
for pkg in docker.io docker-doc docker-compose podman-docker containerd runc; do sudo apt-get remove $pkg; done

sudo apt-get update
sudo apt-get install ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt update -y && sudo apt upgrade -y

sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```
### Test docker functionality

- Docker version check :

```
docker --version
```

- Docker run test :

```
docker run hello-world
```
  
### Step 2 : Create chromium path and install chromium

```
mkdir chromium
cd chromium
```

- check VPS timezone (neccessary)

```
realpath --relative-to /usr/share/zoneinfo /etc/localtime
```

### Step 3 : Create docker compose file :

```
nano docker-compose.yaml
```
-  You'll edit this part of the file below and paste in the yaml file.

```CUSTOM_USER & PASSWORD```: Replace your favorite credentials to login to chromium
```TZ```: Replace with your server timezone
CHROME_CLI: The main page when you open browser
ports: You can replace ```3010 & 3011``` if they have conflict with other running proccesses

- Paste this edited with your details in the file.

```
services:
  chromium:
    image: lscr.io/linuxserver/chromium:latest
    container_name: chromium
    security_opt:
      - seccomp:unconfined #optional
    environment:
      - CUSTOM_USER= #Replace with username
      - PASSWORD= #Replace with password
      - PUID=1000
      - PGID=1000
      - TZ=Europe/London
      - CHROME_CLI=https://github.com/bigray0x
    volumes:
      - /root/chromium/config:/config
    ports:
      - 3010:3000   
      - 3011:3001 
    shm_size: "1gb"
    restart: unless-stopped
```
- Save the file using ctrl + x & y & enter.

### Step 4: Run chromium :

```
cd $HOME && cd chromium

docker compose up -d
```
- The chromium application can be accessed by going to one of these addresses in your local PC browser.

http://your_VPS_IP:3010/
https://your_VPS_IP:3011/

<img width="645" alt="Screenshot 2025-06-14 at 7 16 13 AM" src="https://github.com/user-attachments/assets/9a65f2a6-0b62-44e6-98b1-83379d3dfefa" />


# Step 5: Manage Chromium

```
docker stop chromium
docker rm chromium
docker system prune
```

- this guide was sourced from 0xmoei.
- you can follow my [Github](https://github.com/bigray0x) and My [Twitter](https://x.com/bigray0x)

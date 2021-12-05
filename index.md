## Instructions Below

Fist we must create a Droplet in DigitalOcean.
When creating your droplet, choose Ubuntu 20.04 and whatever specs you want.
Select the data ceneter you wish to use.

Once we creat the droplet, we will run our commands in the droplet console.
To find the droplet console, look at the column on the left hand side and click on Droplets.
Select the droplet you wish to use then click on the Acess page.
Once inside the Access page, there will be an icon that says "Launch Droplet Console"
Now you will be in the locaiton where you can run the following commands.


### Install Docker

First we need to install the tools for the project:

```markdown
sudo apt install apt-transport-https ca-certificates curl software-properties-common -y
```

Then we must add the docker key:
```markdown
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

Next we must add the docker repository:
```markdown
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
```

Now we must switch it to the correct repository:
```markdown
sudo apt install docker-ce -y
```

Now just install docker-compose:
```markdown
sudo curl -L "https://github.com/docker/compose/releases/download/1.27.4/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

Finally set the permission for docker-compose:
```markdown
sudo chmod +x /usr/local/bin/docker-compose
```

### Wireguard Setup

We will continue to run all of the commands in the droplet console.

First we must create a directory for wireguard and config:
```markdown
mkdir -p ~/wireguard/
mkdir -p ~/wireguard/config/
```

Next we must create the docker-comise.yml file and edit it.
I used vim, but feel free to use your text editor of choice:
```markdown
vim ~/wireguard/docker-compose.yml
```

Now that you are editing the docker-compose.yml file, you must copy and paste the content below:
```markdown
version: '3.8'
services:
  wireguard:
    container_name: wireguard
    image: linuxserver/wireguard
    environment:
      - PUID=1000
      - PGID=1000
      - TZ=America/Chicago
      - SERVERURL=x.x.x.x
      - SERVERPORT=51820
      - PEERS=Phone
      - PEERDNS=auto
      - INTERNAL_SUBNET=10.0.0.0
    ports:
      - 51820:51820/udp
    volumes:
      - type: bind
        source: ./config/
        target: /config/
      - type: bind
        source: /lib/modules
        target: /lib/modules
    restart: always
    cap_add:
      - NET_ADMIN
      - SYS_MODULE
    sysctls:
      - net.ipv4.conf.all.src_valid_mark=1
```
Now go and edit the following:
Set the timezone (TZ) to your timezone.
Insert the IP address for your server on the SERVERURL line.
Also, for myself, I only have my Phone as a peer, but if you dersire more then add them on the PEERS line.

Next you will want to navagiate to your wireguard directory:
```markdown
cd ~/wireguard/
```

Then you will start wireguard:
```markdown
docker-compose up -d
```

If you want to connect your phone to wireguard then download the wireguard app from the IOS or Google Play app store.
Once installed, run this command:
```markdown
docker-compose up -d
```
A QR code will be generated that you will be able to scan. Once scanned, you will be able to use the VPN from your phone.

![VPN Test](/CS_VPN_Project/docs/Screenshot_20211204-180015_Chrome.jpg)


### Jekyll Themes



### Support or Contact

Having trouble with Pages? Check out our [documentation](https://docs.github.com/categories/github-pages-basics/) or [contact support](https://support.github.com/contact) and we’ll help you sort it out.

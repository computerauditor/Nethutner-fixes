configuration of kali 

nvm installed node 22 and 20 with pnpm, yarn and npm didnt worked 
i let it installed 

```
(kali-localhost)-[~/automation]
$ nodejs -v ; npm -v ; pnpm -v                                                                             
v20.19.4
10.8.2
10.18.1
```
```n8n version 1.114.4```

### INSTALLATION  

```
sudo apt install npm -y 
```
it install npm as well as node (I am sure it installed node version 20 LTS)

cleared cache and ran npm with ```--unsafe-perm``` flag as root user 
```
sudo rm -rf /root/.npm/_cacache
sudo npm cache clean --force
sudo mkdir -p /root/.npm/_cacache
sudo chmod -R 777 /root/.npm
sudo npm install -g n8n --unsafe-perm
```
didn't worked proper so used pnpm

```
mkdir automation
cd automation/
pnpm init
```

```
pnpm add n8n
```

This installs the n8n using pnpm 


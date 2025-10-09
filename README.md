
NOTE : 1. user ```kali``` has ```bash``` as default while user ```root``` has ```zsh``` by default (there to source type zsh/bash first and then use the source ~/.bashrc or zshrc)
       2. default passwd of user ```kali``` is ```kali``` and ```sudo``` passwd is ```toor```

## ****  1. KEX LOGOUT Issue (NO ROOT) *****************  

No password would work root,toor,kali,blank password,vnc passwd so ultimately kill the process of the screensaver/logout

# Try these all at once or one by one (they will only kill the lock screen process)

```
pkill -f xfce4-screensaver || true
pkill -f light-locker || true
pkill -f gnome-screensaver || true
```

# PRO TIPs :
 1. You can also uninstall the screen savour using ```sudo apt remove xfce4-screensaver``` [NOT YET TESTED]
 2. Can also try to disable lock timeout in system settings and startup settings -> Disable ScreenSaver

## ****  2. KEX Sound ENABLE (NO ROOT) ***************** 

Set up pulse audio in TERMUX first 


Step 1 — In termux update the packages and install pulseaudio

```
pkg update
pkg install pulseaudio
nano $PREFIX/etc/pulse/default.pa
```

COMMENT THIS LINE IF NOT ALREADY COMMENTED 
```
#load-module module-native-protocol-tcp
```
Below it paste this line
```
load-module module-native-protocol-tcp auth-ip-acl=127.0.0.1 auth-anonymous=1
```

Open this file
```
nano $PREFIX/etc/pulse/daemon.conf
```

and set the value of exit-idle-time to -1
```
exit-idle-time = -1
```

and then in finally make this as sound.sh in "Termux" only:

sound
```
pulseaudio --start --load="module-native-protocol-tcp auth-ip-acl=127.0.0.1 auth-anonymous=1" --exit-idle-time=-1
```

```
chmod +x sound
```

```
./sound
```

## Then in the Kali Nethunter run this command in the terminal 
 
```
export PULSE_SERVER=127.0.0.1
```


# PRO Tip
-  Can run the "./sound" in termux & "export PULSE_SERVER=127.0.0.1" in the Kali Linux through some startup script in termux only.
say ```alias nh kex = ./sound ; nh kex; export PULSE_SERVER=127.0.0.1``` something like that


## ****  3. KEX VNC Stopped *****************

The kali vnc stops and shows error:

ERROR
```
vncserver: No matching VNC server running for this user!
vncserver: No matching VNC server running for this user!

Error starting the KeX server.
Please try "nethunter kex kill" or restart your termux session and try again.
```
The issue is due to the xstartup file ```cd /home/kali/.vnc``` cp the logs to ```/Desktop``` for reference and chatgpt if needed
run ```nh -r``` in "Termux" and set a ```kex passwd``` , basically set a kex client on the through sudo user it will run successfully cp the xstart file from the ```/root/.vnc``` and paste it at ```/home/kali/.vnc``` 

# Additionally if it doesn't work then 
------------------------------------------------------------------------------------------------
* INSIDE TERMUX :- 
- make a backup of ```nethunter``` script inside ```data/data/com.termux/files/usr/bin```
- Get the latest nethunter script from ```https://offs.ec/2MceZWr``` copy paste the content as ```nano nethunter``` in data/data/com.termux/files/usr/bin/nethunter and remember to ```chmod +x nethunter```
- It will rerun the installer and restart the nh set up ```DO NOT DELETE THE ROOTFS or the existing installation``` just go on typing ```N``` and go through the set.
   
* INSIDE KALI :-
- make a backup of kex file if any inside ```data/data/com.termux/files/home/kali-arm64/usr/bin```
- ```nano kex``` and paste the content of kex inside it
- ```chmod +x kex```
- and place it in the /usr/bin folder

* Then became the root user by ```su``` in kali or  ```nh -r``` from termux  

Search for this file ```tmp/.X11``` and check its ownership & permisions 

```
ls -ld /tmp/.X11-unix /tmp
```

* Once you're root, run:
```
chown root:root /tmp
chown root:root /tmp/.X11-unix
chmod 1777 /tmp
chmod 1777 /tmp/.X11-unix
```

* show the new ownership/permissions
```
ls -ld /tmp /tmp/.X11-unix
```
------------------------------------------------------------------------------------------------

# Points to Remember :
- Dev Options -> Disable child process
- Force stop the termux and clear cache
- Reset the kex passwd in kali user as ```nh kex passwd```

### ALTERNATE FIXES (KEX VNC ) if the above fixes doesn't worked ---------------------------------------------------------------------

## FIX 1
type ```sudo su``` became the root usr and delete the ```rm root/.vnc/xstartup``` if that doesnt work remove the .vnc folder as well in the root ```rm -rf root/.vnc``` 

Then reset the kali user passwd ```kex passwd``` , force stop termux and clear cache and then re-launch 

Try ```vncserver -depth 24 -geometry 1920x1080```
Then again reset the ```kex passwd```

## FIX 2 (if #1 doesnt work)

FOLLOW : 
```https://github.com/aioont/kex```

## FIX 3 (if #1 & #2 doesnt work)

```
cd ~/.vnc/
rm -rf *.log *.pid
cd /
rm -rf /tmp/.X* /tmp/.xfsm* /tmp/.l2s* /tmp/.ICE-unix
vncserver -kill :*
vncserver -depth 24 -geometry 1920x1080 :1
```

END ALTERNATE FIXES ------------------------------------------------------------------------------

## Pro Tip :

- Can read the logs at ```cd /home/kali/.vnc/``` then ```ls``` , usually a ```localhost:1.log``` file is present , fed it to GPT and ask the issue from the last working Kex server log!
- In Dev Options of Android disable child process restrictions

## ****  4. KEX Default SU Passwd ***************** 

When typing ```su``` default passwd will be ```root``` and not kali 
can change it to kali using terminal commands

## ****  5. Change between kali and root ***************** 

just write ```su kali``` to switch to kali and ```su``` or ```sudo su``` to switch to root

## ****  6. KEX/Termux stopped status 9 (Crashed) *****************  

On Android 12 and above this error is common status-9 crash , 
- Enable developers mode in the system settings of Android
- In the Dev Options -> Disable Child Process Restrictions and then re-start the termux app , all will work fine.

## ****  7. n8n installation ***************** 

⚠️ NEVER use the package name as the directory 
eg : do not ```mkdir n8n``` ❌
            ```mkdir n8n-test```✅

🔹 Step 0: Remove previous nvm/node/pnpm leftovers

```
# Remove nvm folder (if it exists)
rm -rf ~/.nvm

# Remove pnpm global folder (optional, cleans previous installs)
rm -rf ~/.local/share/pnpm

# Remove n8n globally (if installed via pnpm)
pnpm remove -g n8n || true

# Remove pnpm itself
npm uninstall -g pnpm || true

# Remove old n8n data (optional)
rm -rf ~/.n8n


```
### Run as NON-ROOT user (kali) herein and not (root)

🔹 Step 1: Install nvm (Node Version Manager)

```
mkdir -p ~/.nvm
curl -fsSL https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.4/install.sh | bash
```
This installs nvm to ~/.nvm and updates some shell profile files (~/.bashrc or ~/.zshrc) automatically.

NOTE : user ```kali``` has ```bash``` as default while user ```root``` has ```zsh``` by default

🔹 Step 2: Load nvm in the current terminal ie copy paste this in the zshrc and bashrc

```
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"       # loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"   # optional autocomplete
```
Right after installing, your terminal doesn’t yet “know” about nvm. Load it manually:

🔹 Step 3: Test if nvm works

```
┌──(kali㉿localhost)-[~/Desktop/n8n-test]
└─$ nvm --version

0.39.4
```       
🔹 Step 4 — Install Node.js v20
### COMMAND
```
nvm install 20
```
### OUTPUT
```
┌──(kali㉿localhost)-[~/Desktop/n8n-test]
└─$ nvm install 20

Downloading and installing node v20.19.5...
Downloading https://nodejs.org/dist/v20.19.5/node-v20.19.5-linux-arm64.tar.xz...
############################################################################################################################################ 100.0%
Computing checksum with sha256sum
Checksums matched!
Now using node v20.19.5 (npm v10.8.2)
Creating default alias: default -> 20 (-> v20.19.5)
```

### COMMAND

After installation, set it as the default: [OPTIONAL as if nothing is install it would by default itself]

``` 
nvm alias default 20
```

```
┌──(kali㉿localhost)-[~/Desktop/n8n-test]
└─$ nvm alias default 20

default -> 20 (-> v20.19.5)
```
🔹 Step 5

Now switch to it immediately in this terminal:

```
nvm use 20
```

check 
### COMMAND 

```
node -v
npm -v
```

### OUTPUT
```
┌──(kali㉿localhost)-[~/Desktop/n8n-test]
└─$ node -v
npm -v

v20.19.5
10.8.2
```

🔹 Step 6 Install other important dependancies (Install build tools (needed for SQLite))

1. Build tools and SQLite
```
sudo apt update
sudo apt install -y build-essential python3 g++ make pkg-config libsqlite3-dev python-is-python3
```

2. Then tell npm which Python to use:
```
echo 'export PYTHON=/usr/bin/python3' >> ~/.zshrc 
source ~/.zshrc
```

3. Install pnpm globally

```
npm install -g pnpm
```
Check it:
```
└─$ pnpm -v
10.18.1
```
Step 4 — Install n8n globally

```
pnpm add -g n8n
```

### IF ERROR 
We need to ensure pnpm’s global bin directory is set and in your PATH, otherwise installing n8n globally will fail like before.

Step 1 — Find the pnpm global bin folder
```
pnpm bin -g
```

Step 2 — Add pnpm global bin to your PATH
```
export PNPM_HOME="$(pnpm bin -g)"
export PATH="$PNPM_HOME:$PATH:$HOME/.local/bin"
```
### OUTPUT
```
┌──(kali㉿localhost)-[~/Desktop/n8n-test]
└─$ pnpm bin -g

/home/kali/.local/share/pnpm
                                                                                                                                                   
┌──(kali㉿localhost)-[~/Desktop/n8n-test]
└─$ export PNPM_HOME="$(pnpm bin -g)"
export PATH="$PNPM_HOME:$PATH:$HOME/.local/bin"
```

Step 3 — Make it permanent

```
echo 'export PNPM_HOME="$(pnpm bin -g)"' >> ~/.zshrc
echo 'export PATH="$PNPM_HOME:$PATH:$HOME/.local/bin"' >> ~/.zshrc
source ~/.zshrc
```

Try again to Install n8n globally
```
pnpm add -g n8n
```
### or 
```
pnpm set registry https://registry.npmjs.org/
pnpm add n8n --no-optional
```
In many cases few dependancies will be missing

Try adding it mannually :
step 1/2: Mannual install xlsx @0.20.2
```
┌──(kali㉿localhost)-[~/Desktop/n8n-test]
└─$ pnpm add xlsx@0.20.2

 ERR_PNPM_NO_MATCHING_VERSION  No matching version found for xlsx@0.20.2 while fetching it from https://registry.npmjs.org/
This error happened while installing a direct dependency of /home/kali/Desktop/n8n-test
The latest release of xlsx is "0.18.5".
If you need the full list of all 108 published versions run "$ pnpm view xlsx versions".
```

step 2/2: So install the latest available xlsx ie @ 0.18.5
```
┌──(kali㉿localhost)-[~/Desktop/n8n-test]
└─$ pnpm add xlsx@0.18.5
Packages: +9
+++++++++
Progress: resolved 9, reused 0, downloaded 9, added 9, done

dependencies:
+ xlsx 0.18.5

Done in 3.7s using pnpm v10.18.1

```
Now reinstall the n8n 

### DIDN'T worked so 

revised step 1/2: Forces pnpm to download all dependencies from npm registry, not the CDNs.
```
pnpm add -g n8n --registry=https://registry.npmjs.org/

```
— Download missing packages manually
```
Download xlsx-0.20.2.tgz manually:

wget https://cdn.sheetjs.com/xlsx-0.20.2/xlsx-0.20.2.tgz -O ~/xlsx-0.20.2.tgz


Install it with pnpm:

pnpm add -g ~/xlsx-0.20.2.tgz


Retry installing n8n:

pnpm add -g n8n
```

```
pnpm add -g n8n
```

Check it:
```
which n8n
n8n --version
```

### Configuration of kali 

### INSTALLATION  

```
sudo apt install npm -y 
```
it install npm as well as node (I am sure it installed node version 20 LTS)

```
$ nodejs -v ; npm -v ; pnpm -v     

v20.19.4
9.2.0
10.18.2
```

cleared cache and ran npm with ```--unsafe-perm``` flag as root user 
```
sudo rm -rf /root/.npm/_cacache
sudo npm cache clean --force
sudo mkdir -p /root/.npm/_cacache
sudo chmod -R 777 /root/.npm
# sudo npm install -g n8n --unsafe-perm
```
didn't worked proper so used pnpm

## LOCALLY
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------
```
mkdir automation
cd automation/
pnpm init
```

```
pnpm add n8n
```
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  OR if wants global installation 

## GLOABALLY 

----------------------------------------------------------------------------------------------------------------------------------------------------------------------------
```
pnpm setup
```
```
pnpm add n8n -g
pnpm add sqlite3 -g
pnpm add xlsx@0.18.5 -g      # OPTIONAL if it shows some excel error then use this 
```
all installed successfully  

```
cd /home/kali/.local/share/pnpm/global/5/node_modules
```

```
┌──(kali㉿localhost)-[~/.local/share/pnpm/global/5/node_modules]
└─$ ls
n8n  sqlite3  xlsx
```
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------

This installs the n8n using pnpm either locally or gloablly 

```
pnpm exec n8n
```

ERROR

```
No encryption key found - Auto-generating and saving to: /home/kali/.n8n/config
Permissions 0660 for n8n settings file /home/kali/.n8n/config are too wide. This is ignored for now, but in the future n8n will attempt to change the permissions automatically. To automatically enforce correct permissions now set N8N_ENFORCE_SETTINGS_FILE_PERMISSIONS=true (recommended), or turn this check off set N8N_ENFORCE_SETTINGS_FILE_PERMISSIONS=false.
Initializing n8n process
There was an error initializing DB
SQLite package has not been found installed. Try to install it: npm install sqlite3 --save
```

chmod for permissions -- fixes permisions
```
chmod 600 ~/.n8n/config || true   # create file first if it doesn't exist: touch ~/.n8n/config && chmod 600 ~/.n8n/config
export N8N_ENFORCE_SETTINGS_FILE_PERMISSIONS=true
# To persist, add the export to ~/.profile or your shell rc
```
PERMISSION ERROR FIXED!!!

### SQLite error :

pnpm stores global modules in a content-addressed store and creates symlinks in:
```
~/.local/share/pnpm/global/5/node_modules
```
However, when n8n runs, it expects to require('sqlite3') in its own node_modules, not globally.
Because pnpm isolates packages, n8n can’t see sqlite3 even though it’s installed globally.

```https://stackoverflow.com/questions/58317165/installing-sqlite3-for-nodejs```
(also refer ```https://gist.github.com/ScottJWalter/7a44d6d081ec065f1df7ea5aea72edf4``` excelent script)

```
npm cache clean
npm install -g npm
sudo  npm install -g npm
npm install  -g node-gyp
npm install --save sqlite3
```



















So tried ```npm install sqlite3 --save``` didn't worked hence again tried pnpm 

Install sqlite3 in the same working directory ie: /automation 
```
pnpm add sqlite3
```

```
.....
dependencies:
+ sqlite3 5.1.7
Done in 1m 54.2s using pnpm v10.18.1
```
```
tree
```
```
(kali?localhost)-[~/automation]
$ tree
.
??? node_modules
?�� ??? n8n -> .pnpm/n8n@1.114.4_0725ed04a2a2a35c694575b9b91a2375/node_modules/n8n
?�� ??? sqlite3 -> .pnpm/sqlite3@5.1.7/node_modules/sqlite3
??? package.json
??? pnpm-lock.yaml
4 directories, 2 files
```

# Then rebuild SQLite3
```
sudo apt update
sudo apt install -y build-essential python3 make g++ libsqlite3-dev
```

Rebuid using pnpm [OPTIONAL -> Not Worked]
```
pnpm add sqlite3 --save            # ensure sqlite3 is a dependency in package.json (not just in pnpm store)
pnpm rebuild sqlite3               # rebuild native modules (recompiles for your current Node)
 OR 
pnpm rebuild                       # or rebuild all native modules
```
COMMAND
```
node -v
pnpm -v
which node
command -v n8n
pnpm root -g
```
OUTPUT
```
v20.19.4
10.18.2
/usr/bin/node
/root/.local/share/pnpm/global/5/node_modules
```
So cd to the node_modules dir and rebuild sqlite3 if you find it
```
cd /root/.local/share/pnpm/global/5/node_modules
```
This dir stores the global binaries i think , so add the sqlite3 binary herein 

If already not setup pnpm then set it up now
```
pnpm setup
```
This will:

Create the global package directory (usually ~/.local/share/pnpm)
Set the PNPM_HOME environment variable (Add it to your shell’s PATH (in ~/.bashrc, ~/.zshrc, etc.) . Then source ~/.bashrc and ~/.zshrc

```
pnpm add -g sqlite3
```

OUTPUT
```
┌──(kali㉿localhost)-[~]
└─$ pnpm install sqlite3 -g
/home/kali/.local/share/pnpm/global/5:
+ sqlite3 5.1.7

╭ Warning ──────────────────────────────────────────────────────────────────────────────────────╮
│                                                                                               │
│   Ignored build scripts: sqlite3.                                                             │
│   Run "pnpm approve-builds -g" to pick which dependencies should be allowed to run scripts.   │
│                                                                                               │
╰───────────────────────────────────────────────────────────────────────────────────────────────╯
Done in 8.5s using pnpm v10.18.1
```






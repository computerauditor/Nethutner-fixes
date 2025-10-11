### Configuration of kali 

⚠️ NEVER use the package name as the directory 
eg : do not ```mkdir n8n``` ❌
            ```mkdir n8n-test```✅
            ```mkdir autoation```✅ (herein we will create a dir at /Desktop or /kali )

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
sudo rm -rf /root/.npm/_cacache            # or kali if kali user
sudo npm cache clean --force               # or kali if kali user 
sudo mkdir -p /root/.npm/_cacache          # or kali if kali user
sudo chmod -R 777 /root/.npm               # or kali if kali user
# sudo npm install -g n8n --unsafe-perm
```
didn't worked proper so used pnpm

NOTE : The following error is a NETWORK ERROR switch to a better wifi or try to increase timeout of the installing package 
```
npm error code ETIMEDOUT
npm error errno ETIMEDOUT
```

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

### NOTE : Try not adding sqlite and xlsx if n8n installs and shows the below output after doing the pnpm add n8n (with/without g) it will show the following in output

```
pnpm exec n8n  # or n8n (if globally)
```

output
```
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++ Progress: resolved 1633, reused 1389, downloaded 218, added 1614, done  WARN  Issues with peer dependencies found . ├─┬ openai 5.12.2 │ └── ✕ unmet peer ws@^8.18.0: found 8.17.1 ├─┬ langchain 0.3.33 │ └─┬ @langchain/openai 0.6.7 │ └─┬ openai 5.23.2 │ └── ✕ unmet peer ws@^8.18.0: found 8.17.1 ├─┬ @browserbasehq/stagehand 1.14.0 │ └── ✕ unmet peer openai@^4.62.1: found 5.12.2 └─┬ n8n 1.114.4 ├─┬ @n8n/db 0.25.1 │ └─┬ @n8n/typeorm 0.3.20-12 │ ├── ✕ unmet peer mongodb@^5.8.0: found 6.11.0 │ └── ✕ unmet peer @sentry/node@<=8.x: found 9.46.0 ├─┬ @n8n/n8n-nodes-langchain 1.113.1 │ └─┬ @langchain/community 0.3.50 │ ├── ✕ unmet peer mongodb@^6.17.0: found 6.11.0 │ └── ✕ unmet peer @qdrant/js-client-rest@^1.15.0: found 1.14.1 └─┬ @rudderstack/rudder-sdk-node 2.1.4 └── ✕ unmet peer tslib@2.6.2: found 2.8.1 dependencies: + n8n 1.114.4 ╭ Warning ───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────╮ │ │ │ Ignored build scripts: @parcel/watcher, @scarf/scarf, @sentry-internal/node-native-stacktrace, cpu-features, msgpackr-extract, │ │ protobufjs, sqlite3, ssh2. │ │ Run "pnpm approve-builds" to pick which dependencies should be allowed to run scripts. │ │ │ ╰────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯ Done in 4m 54.4s using pnpm v10.18.2
```

This installs the n8n using pnpm either locally or gloablly 
```
pnpm approve-builds
```
press ```a``` and then ```ENTER``` to approve all the builds run again you will be greated with the following errors

ERROR OUTPUT
```
┌──(kali㉿localhost)-[~/automation] └─$ pnpm exec n8n Initializing n8n process n8n ready on ::, port 5678 There are deprecations related to your environment variables. Please take the recommended actions to update your configuration: - DB_SQLITE_POOL_SIZE -> Running SQLite without a pool of read connections is deprecated. Please set DB_SQLITE_POOL_SIZE to a value higher than zero. See: https://docs.n8n.io/hosting/configuration/environment-variables/database/#sqlite - N8N_RUNNERS_ENABLED -> Running n8n without task runners is deprecated. Task runners will be turned on by default in a future version. Please set N8N_RUNNERS_ENABLED=true to enable task runners now and avoid potential issues in the future. Learn more: https://docs.n8n.io/hosting/configuration/task-runners/ - N8N_BLOCK_ENV_ACCESS_IN_NODE -> The default value of N8N_BLOCK_ENV_ACCESS_IN_NODE will be changed from false to true in a future version. If you need to access environment variables from the Code Node or from expressions, please set N8N_BLOCK_ENV_ACCESS_IN_NODE=false. Learn more: https://docs.n8n.io/hosting/configuration/environment-variables/security/ - N8N_GIT_NODE_DISABLE_BARE_REPOS -> Support for bare repositories in the Git Node will be removed in a future version due to security concerns. If you are not using bare repositories in the Git Node, please set N8N_GIT_NODE_DISABLE_BARE_REPOS=true. Learn more: https://docs.n8n.io/hosting/configuration/environment-variables/security/ [license SDK] Skipping renewal on init: license cert is not initialized double free or corruption (out)  ERR_PNPM_RECURSIVE_EXEC_FIRST_FAIL  Command was killed with SIGABRT (Aborted): n8n
```

Rebuild sqlite to fix double free or corruption (out) error
```
pnpm rebuild sqlite3
```
add better-sqlite3 in the same n8n dir where all packages are located  
```
pnpm add better-sqlite3
```

Find the n8n dir by 

```
which n8n  # usually output (if installed n8n globally) ---> ~/.local/share/pnpm]
```
therein ,
ie: Inside the same n8n dir where all packages are located ```nano .env``` and paste the following code:
```
DB_TYPE=sqlite
DB_SQLITE_POOL_SIZE=5
N8N_RUNNERS_ENABLED=true
N8N_BLOCK_ENV_ACCESS_IN_NODE=false
N8N_GIT_NODE_DISABLE_BARE_REPOS=true
```

then finally 

```
pnpm exec n8n
```

It should work now !!! ✅


### OTHER ERRORS AND POSSIBLE SOLUTIONS :


ERROR

```
No encryption key found - Auto-generating and saving to: /home/kali/.n8n/config
Permissions 0660 for n8n settings file /home/kali/.n8n/config are too wide. This is ignored for now, but in the future n8n will attempt to change the permissions automatically. To automatically enforce correct permissions now set N8N_ENFORCE_SETTINGS_FILE_PERMISSIONS=true (recommended), or turn this check off set N8N_ENFORCE_SETTINGS_FILE_PERMISSIONS=false.
Initializing n8n process
There was an error initializing DB
SQLite package has not been found installed. Try to install it: npm install sqlite3 --save
```

this basically means few permission error so chmod for permissions -- fixes permisions and sqlite is not found by n8n so install it in the same directory of n8n packages
```
chmod 600 ~/.n8n/config || true   # create file first if it doesn't exist: touch ~/.n8n/config && chmod 600 ~/.n8n/config
export N8N_ENFORCE_SETTINGS_FILE_PERMISSIONS=true
# To persist, add the export to ~/.profile or your shell rc
```
PERMISSION ERROR FIXED!!!

### SQLite error :
Can Try :

## Best Way : is after installing  n8n using pnpm ```pnpm approve-builds``` and select all and approve it other possible ways are as below :

```
pnpm add sqlite3 -g # or pnpm add sqlite3 (inside the n8n dir where all packages are located and then may have to rebuild it)
```

pnpm stores global modules in a content-addressed store and creates symlinks in:
```
~/.local/share/pnpm/global/5/node_modules
```
However, when n8n runs, it expects to require('sqlite3') in its own node_modules, not globally.
Because pnpm isolates packages, n8n can’t see sqlite3 even though it’s installed globally.


```
https://stackoverflow.com/questions/58317165/installing-sqlite3-for-nodejs
```

```
cd n8n 
```

ie: ```cd ~/.local/share/pnpm/global/5/node_modules/n8n```
and inside this add sqlite3

```pnpm add sqlite3```
then 
```
pnpm approve-builds
```
then approve all the packages ```press a``` and then ```ENTER``` to approve all the packages

RUN n8n
```n8n```

ERROR
```
nodes package n8n-nodes-base is already loaded.
 Please delete this second copy at path /home/kali/automation/node_modules/.pnpm/n8n@1.114.4_4636955cc7c9a683d35ce5b4c22bfc3e/node_modules/n8n/node_modules/n8n-nodes-base
nodes package n8n-nodes-base is already loaded.
 Please delete this second copy at path /home/kali/automation/node_modules/.pnpm/n8n@1.114.4_4636955cc7c9a683d35ce5b4c22bfc3e/node_modules/n8n/node_modules/n8n-nodes-base
nodes package @n8n/n8n-nodes-langchain is already loaded.
 Please delete this second copy at path /home/kali/automation/node_modules/.pnpm/n8n@1.114.4_4636955cc7c9a683d35ce5b4c22bfc3e/node_modules/n8n/node_modules/@n8n/n8n-nodes-langchain
nodes package @n8n/n8n-nodes-langchain is already loaded.
 Please delete this second copy at path /home/kali/automation/node_modules/.pnpm/n8n@1.114.4_4636955cc7c9a683d35ce5b4c22bfc3e/node_modules/n8n/node_modules/@n8n/n8n-nodes-langchain

```
SOLUTIONS :
```
rm /home/kali/automation/node_modules/.pnpm/n8n@1.114.4_4636955cc7c9a683d35ce5b4c22bfc3e/node_modules/n8n/node_modules/n8n-nodes-base
rm /home/kali/automation/node_modules/.pnpm/n8n@1.114.4_4636955cc7c9a683d35ce5b4c22bfc3e/node_modules/n8n/node_modules/n8n-nodes-base
rm /home/kali/automation/node_modules/.pnpm/n8n@1.114.4_4636955cc7c9a683d35ce5b4c22bfc3e/node_modules/n8n/node_modules/@n8n/n8n-nodes-langchain
rm /home/kali/automation/node_modules/.pnpm/n8n@1.114.4_4636955cc7c9a683d35ce5b4c22bfc3e/node_modules/n8n/node_modules/@n8n/n8n-nodes-langchain
```


🔹 Step 0: Remove previous nvm/node/pnpm leftovers

```
# Remove nvm folder (if it exists)
rm -rf ~/.nvm

# Remove pnpm global folder (optional, cleans previous installs)
rm -rf ~/.local/share/pnpm

# Remove n8n globally (if installed via pnpm)
pnpm remove -g n8n || true

```
### Run as NON-ROOT user (kali) herein and not (root)

🔹 Step 1: Install nvm (Node Version Manager)

```
mkdir -p ~/.nvm
curl -fsSL https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.4/install.sh | bash
```
This installs nvm to ~/.nvm and updates some shell profile files (~/.bashrc or ~/.zshrc) automatically.

🔹 Step 2: Load nvm in the current terminal

```
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"       # loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"   # optional autocomplete
```
Right after installing, your terminal doesn’t yet “know” about nvm. Load it manually:

🔹 Step 3: Test if nvm works

```
┌──(kali㉿localhost)-[~/Desktop/n8n]
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
┌──(kali㉿localhost)-[~/Desktop/n8n]
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

After installation, set it as the default:

``` 
nvm alias default 20
```

```
┌──(kali㉿localhost)-[~/Desktop/n8n]
└─$ nvm alias default 20

default -> 20 (-> v20.19.5)
```



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
mkdir /home/kali/.nvm  
curl -fsSL https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.4/install.sh | bash
```
This installs nvm to ~/.nvm and updates some shell profile files (~/.bashrc or ~/.zshrc) automatically.

🔹 Step 2: Load nvm in the current terminal

```
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
```
Right after installing, your terminal doesn’t yet “know” about nvm. Load it manually:


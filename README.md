# WSL2 Setup Guide

This guide will walk you through setting up WSL2 on your machine for Development.

## Step 1: Create .wslconfig

Create a file called `.wslconfig` on your `C:\Users\User` directory with the following configuration:$

```ini
[wsl2]
memory=8GB
localhostForwarding=true

[experimental]
autoMemoryReclaim=gradual
```

If you have 32GB ram, I suggest setting 8GB, or more if you would like.
If you have 16GB, I would go for 4GB ram.

## Step 2: Install Necessary Packages

Open your WSL2 terminal and run the following commands:

```bash
sudo apt-get update && sudo apt-get upgrade -y
sudo apt-get install git curl zsh python3 python3-pip build-essential -y
```

When prompted, press "Y" to set zsh as the default shell.

## Step 3: Install Oh My Zsh

Run the following command to install Oh My Zsh:

```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

## Step 4: Setup Directories and Syntax Highlighting

```bash
mkdir Documents Downloads Stuff && mkdir Documents/GitHub
cd Stuff
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git
echo "source ${(q-)PWD}/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh" >> ${ZDOTDIR:-$HOME}/.zshrc
source ./zsh-syntax-highlighting/zsh-syntax-highlighting.zsh
cd ..
```

## Step 5: Install Homebrew

Run the following command to install Homebrew

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

When prompted, press RETURN/ENTER.

Then, add Homebrew to your shell environment:

```bash
(echo; echo 'eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"') >> /home/${USER}/.zshrc
eval "$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)"
```

## Step 6: Install Additional Tools

Use Homebrew to install additional tools:

```bash
brew install neovim tmux fastfetch go nvm
```

## Step 7: Setup NVM

Create a directory for NVM and add it to your shell profile:

```bash
mkdir ~/.nvm
```

Add the following to your shell profile e.g. `~/.profile` or `~/.zshrc`:

```bash
export NVM_DIR="$HOME/.nvm"
[ -s "/home/linuxbrew/.linuxbrew/opt/nvm/nvm.sh" ] && \. "/home/linuxbrew/.linuxbrew/opt/nvm/nvm.sh"  # This loads nvm
[ -s "/home/linuxbrew/.linuxbrew/opt/nvm/etc/bash_completion.d/nvm" ] && \. "/home/linuxbrew/.linuxbrew/opt/nvm/etc/bash_completion.d/nvm"  # This loads nvm bash_completion
```

Once added, run:

```bash
source ~/.zshrc
```

Then, install the latest LTS version of Node.js:

```bash
nvm install --lts
```

## Step 8: Setup Git

Generate a new SSH key and set your Git config:

```bash
ssh-keygen -t ed25519 -C "your_email@example.com"
git config --global user.name "Your Name"
git config --global user.email "your_email@example.com"
```

Then, add your new SSH key to your GitHub account:

```bash
cat .ssh/id_ed25519.pub
```

Go to <a href="https://github.com/settings/keys">GitHub SSH Keys</a>, create a new key with whatever name you want, like "WSL", and copy and paste the SSH key.

## Setup 9: Setup VS Code

Install VS Code on your machine and download the WSL extension. Go to a directory like `Documents/GitHub` and run "code ."

```bash
cd ~/Documents/GitHub && code .
```

This will download VS Code Server.

## Step 10: Install MongoDB Community Server on WSL

RUn the following commands to install MongoDB:

```bash
sudo apt-get install gnupg curl
curl -fsSL https://www.mongodb.org/static/pgp/server-7.0.asc | \
   sudo gpg -o /usr/share/keyrings/mongodb-server-7.0.gpg \
   --dearmor
echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-7.0.gpg ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/7.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-7.0.list
sudo apt-get update
sudo apt-get install -y mongodb-org
sudo systemctl start mongod
sudo systemctl enable mongod
```

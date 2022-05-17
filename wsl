# Install 
> https://docs.microsoft.com/en-us/windows/wsl/install

- open windows Terminal (Powershell) **as administrator**
- run `wsl --install`
- restart (if wasn't already installed)
- run `wsl --set-default-version 2`
- run `wsl --update`
- run `wsl --shutdown` to restart WSL
- ~~run `wsl --install -d Ubuntu-20.04` to install Ubuntu 20.04 LTS distribution~~
- ~~give up and~~ get Ubuntu 22.04 LTS from the windows store
- enter a username and password
- run `sudo apt update` in the terminal that opens
- run `sudo apt upgrade`

# Install NodeJS
```sh
# curl is probably already installed, but just in case
# install curl
sudo apt-get install curl

# Install nvm
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | bash

# check your .bashrc for the nvm aliases listed in the command output, and add them if missing
nano ~/.bashrc

# reload .bashrc if you added aliases
source ~/.bashrc

# Install node
nvm install node
```

# Navigate to Linux via Windows (Powershell)

```bash
cd \\wsl.localhost\Ubuntu-22.04\home
pwd

Path
----
Microsoft.PowerShell.Core\FileSystem::\\wsl.localhost\Ubuntu-22.04\home
```

Outside of Windows Terminal's Powershell (i.e., Kraken terminal) use full path to `cd` to Linux
```bash
# In Git Kraken 

# Won't work
cd \\wsl.localhost\Ubuntu-22.04\home

# Works
cd Microsoft.PowerShell.Core\FileSystem::\\wsl.localhost\Ubuntu-22.04\home
```

## Open dir in explorer window

```bash
cd ~/code
explorer.exe
```

# Navigate to Windows via Linux

```bash
# switch to the mounted directory
cd /mnt

# switch to the Drive you want to navigate
cd c
```

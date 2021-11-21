My configuration for Fedora vm.
Install Neovim and Tmux configurations
```bash
curl https://raw.githubusercontent.com/vadxx/config/main/setup.sh | bash
```

# Tmux
The ` is prefix. Added keybinds:
Keys | Desc
---|---
prefix + k | kill panel
prefix + v | split vertical
prefix + h | split horizontal


# NeoVim
Added useful keybind `;` as `:`. Added keybinds:

Keys | Desc
---|---
prefix + t | new tab
prefix + - | split vertical
prefix + \ | split horizontal
visual mode + > | move selected lines right
visual mode + < | move selected lines left

# Core

```bash
# dnf
echo "max_parallel_downloads=10" | sudo tee -a /etc/dnf/dnf.conf
echo "fastestmirror=True" | sudo tee -a /etc/dnf/dnf.conf

# vbox
sudo usermod -aG vboxsf $USER
sudo ln -s /media/sf_G_DRIVE /vm

# terminal
sudo dnf install -y fish util-linux-user neovim
chsh -s /usr/bin/fish
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/JetBrains/JetBrainsMono/master/install_manual.sh)"

# replace vi and vim to nvim
sudo ln -s /usr/bin/nvim /usr/local/bin/vi 
sudo ln -s /usr/bin/nvim /usr/local/bin/vim

# ssh
systemctl start sshd.service
systemctl enable sshd.service
```
Do reboot `systemctl reboot` after Core Setup.

## SSH forward
Open the Settings for your VirtualBox machine and add configuration for forwarding ports as on the screenshot  
![Forward port](./assets/vbox-ssh.png)  
Open your Windows powershell and do next.
```powershell
mkdir .ssh
ssh-keygen -t rsa -b 4096 -f "$HOME\.ssh\id_rsa-remote-ssh"
# press Enter multiple times
# then create config file for ssh using notepad.exe
New-Item -Path $HOME\.ssh\config -ItemType File
notepad.exe $HOME\.ssh\config
# write these lines
Host vm
 User user
 HostName localhost
 Port 3022
 IdentityFile ~/.ssh/id_rsa-remote-ssh 

# save it and exit from notepad
# auth your key
$USER_AT_HOST="user@localhost"
$PUBKEYPATH="$HOME\.ssh\id_rsa-remote-ssh.pub"
$pubKey=(Get-Content "$PUBKEYPATH" | Out-String); ssh -p 3022 "$USER_AT_HOST" "mkdir -p ~/.ssh && chmod 700 ~/.ssh && echo '${pubKey}' >> ~/.ssh/authorized_keys && chmod 600 ~/.ssh/authorized_keys"
# type `yes` and write your linux password
# check connection in windows terminal
ssh vm
```



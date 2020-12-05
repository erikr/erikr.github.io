---
layout: post
title: Linux setup guide
---

## Install basic utilities
```
sudo apt-get install vim git curl zsh tmux tree
```

## Shell, dotfiles, zsh theme

ZSH
```
zsh
chsh -s $(which zsh)
sh -c "$(wget -O- https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

Dotfiles and shell scripts
```
git clone https://github.com/erikr/dotfiles.git  
cd dotfiles && sh generate_symlinks.sh
```

zsh-autosuggestions
```
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
```

Pure theme
```
mkdir -p "$HOME/.zsh"
git clone https://github.com/sindresorhus/pure.git "$HOME/.zsh/pure"
```
> The rest of the settings are ready in `.zshrc`


ZSH syntax highlighting
```
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
rm -rf zsh-syntax-highlighting
```

Tmux plugin manager (`tpm`)
```
git clone https://github.com/tmux-plugins/tpm ~/.tmux/plugins/tpm
```

## Powerline fonts
```
sudo apt-get install fonts-powerline
```

## Vim plugins

[vim-plug](https://github.com/junegunn/vim-plug/wiki/tutorial)
```
curl -fLo ~/.vim/autoload/plug.vim --create-dirs \
    https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
```

Solarized colors:
```
git clone git@github.com:altercation/vim-colors-solarized.git
cd vim-colors-solarized/colors
mkdir ~/.vim/colors
mv solarized.vim ~/.vim/colors/
cd && rm -rf vim-colors-solarized
```

then start vim and call `:PlugInstall` to install the plugins specified in `.vimrc`:
```
vim -c PlugInstall
```

Then [fix `Black`](https://github.com/psf/black/issues/1379#issuecomment-623731659):
```
cd ~/.vim
black/bin/pip install --upgrade pip
black/bin/pip install --upgrade git+https://github.com/psf/black.git
```

## Miniconda and environments

Download and install
```
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh 
sh Miniconda3
source ~/miniconda3/bin/activate
```

No need to run `conda init zsh` because my `.zshrc` sets paths.

Create environment:
```
conda env create -f ~/dotfiles/environment.yml
```

For a multi-user installation
```
sudo groupadd aguirrelab
sudo chgrp -R aguirrelab /home/aguirrelab/miniconda3
sudo chmod 777 -R /home/aguirrelab/miniconda3
```

Add users to this group:
```
sudo adduser username aguirrelab
```

Users should set up Miniconda on an Aguirre Lab machine by following [these instructions]({% post_url 2020-04-08-miniconda %}).


## SSH
Install some stuff
```
sudo apt install openssh-server
sudo systemctl status ssh
sudo ufw allow ssh
sudo systemctl enable ssh
```

From your local machine, add your public key to the remote machine's authorized keys:
```
cat ~/.ssh/id_rsa.pub | ssh b@B 'cat >> ~/.ssh/authorized_keys'
```

## `gpg`
```
sudo apt install gnupg
gpg --full-generate-key
gpg --list-secret-keys --keyid-format LONG
gpg --armor --export KEY_ID_HERE
```

## Customize the MOTD
Disable the help text that appears when you log in, news, etc.:
```bash
sudo chmod -x /etc/update-motd.d/10-help-text
sudo chmod -x /etc/update-motd.d/50-motd-news
sudo chmod -x /etc/update-motd.d/95-hwe-eol
```

Remove the printing of the last login datetime and IP:

```bash
sudoedit /etc/ssh/sshd_config
```
Find the line that says:

```
PrintLastLog yes
```

and change to

```
PrintLastLog no
```
or add if it doesnt exist.

Restart SSH:

```
service ssh restart
```

## For local use

Set up Solarized Dark for the terminal
```
git clone https://github.com/aruhier/gnome-terminal-colors-solarized.git  
cd gnome-terminal-colors-solarized  
./install.sh  
cd && rm -rf gnome-terminal-colors-solarized
```

Install [JetBrains Mono font](https://www.jetbrains.com/lp/mono/)

Install Dropbox; [refer to this setup guide on our lab wiki](https://github.com/aguirre-lab/aguirre-lab/blob/master/dropbox.md) for instructions.

## For `mithril`:

Edit grub
```
sudo vim /etc/default/grub
--verbose debug nomodeset
```

Create mount points for HDDs, and mount on startup:
```
sudo \mkdir /media/8tb  
sudo \mkdir /media/2tb  
sudo -E vim /etc/fstab  
/dev/sda1 /media/8tb ext4 defaults 0 0
/dev/sdb1 /media/2tb ext4 defaults 0 0
```

Set up CIFS and SMB mount to external servers
```
sudo apt-get install cifs-utils  
sudo \mkdir /media/mad3  
```
Mount scripts are at `~/dotfiles/mount-mad3.sh`

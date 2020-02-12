---
layout: post
title: Linux setup checklist
---

This is mostly for me to remember and copy-paste commands for setting up new workstations. 

```
# Install stuff that cannot be set up from command line
https://www.google.com/chrome/  
https://ulauncher.io    

# Install apps and tools (including ZSH)
sudo apt-get install vim git curl zsh tmux tree  

# Edit grub
sudo vim /etc/default/grub  
> --verbose debug nomodeset

# ZSH
zsh
chsh -s $(which zsh)  

# Oh-My-ZSH
sh -c "$(wget -O- https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"  
  
# Set up dotfiles and shell scripts
mkdir repos && cd repos
git clone https://github.com/erikr/dotfiles.git  
cd dotfiles && sh generate_symlinks.sh  

# Set up Vundle
git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim  
vim +PluginInstall +qall   

# Install TypeWritten ZSH theme
git clone git@github.com:reobin/typewritten.git $ZSH_CUSTOM/themes/typewritten
ln -s "$ZSH_CUSTOM/themes/typewritten/typewritten.zsh-theme" "$ZSH_CUSTOM/themes/typewritten.zsh-theme"  

# Set up Solarized for the terminal
git clone https://github.com/aruhier/gnome-terminal-colors-solarized.git  
cd gnome-terminal-colors-solarized  
./install.sh  
cd && rm -rf gnome-terminal-colors-solarized  

# Set up ZSH syntax highlighting
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting  
rm -rf zsh-syntax-highlighting

# Install JetBrains Mono font
https://www.jetbrains.com/lp/mono/

# Set up SSH to mithril
sudo apt install openssh-server  
sudo systemctl status ssh  
sudo ufw allow ssh  

> May need to remove mithril IP address from ~/.ssh/known_hosts  

cat ~/.ssh/id_rsa.pub | ssh b@B 'cat >> ~/.ssh/authorized_keys'  

# Auto-mount internal HDDs
sudo \mkdir /media/8tb  
sudo \mkdir /media/2tb  
sudo -E vim /etc/fstab  

> add the following to fstab:

/dev/sda1 /media/8tb ext4 defaults 0 0  
/dev/sdb1 /media/2tb ext4 defaults 0 0  

# Set up CIFS and SMB mount to mad3
sudo apt-get install cifs-utils  
sudo \mkdir /media/mad3  
sh ~/repos/dotfiles/mount-mad3.sh  

# Install Anaconda
wget https://repo.anaconda.com/archive/Anaconda3-2019.10-Linux-x86_64.sh
sh Anaconda3
conda env create -f ~/repos/dotfiles/py37.yml

> Set up Anaconda for multiple users by setting install location to:

/opt/anaconda3

source opt/anaconda3/bin/activate

# Install Dropbox
https://www.dropbox.com/install-linux  
> Waiting for the app to ask for the link code never works. Instead, log in at dropbox.com/login.
```

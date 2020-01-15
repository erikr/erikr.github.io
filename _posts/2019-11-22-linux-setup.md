---
layout: post
title: Linux setup checklist
---

```
# Install stuff that cannot be set up from command line
https://www.google.com/chrome/  
https://ulauncher.io    
https://anydesk.com/en  
  

# Install apps and tools
sudo apt-get install vim git curl zsh screen tree  


# Edit grub
sudo vim /etc/default/grub  
> --verbose debug nomodeset


# Oh-My-ZSH
https://github.com/ohmyzsh/ohmyzsh#basic-installation  
sudo reboot  
sudo apt-get install fonts-powerline   
  
# Set up dotfiles and shell scripts
mkdir repos && cd repos
git clone https://github.com/erikr/dotfiles.git  
cd dotfiles && sh create_symlinks.sh  

# Install TypeWritten ZSH theme
git clone https://github.com/erikr/typewritten.git $ZSH_CUSTOM/themes/typewritten
ln -s "$ZSH_CUSTOM/themes/typewritten/typewritten.zsh-theme" "$ZSH_CUSTOM/themes/typewritten.zsh-theme"  
vim ~/repos/dotfiles/.zshrc  
add ZSH_THEME="typewritten/typewritten"  

# Set up Solarized for the terminal
git clone https://github.com/aruhier/gnome-terminal-colors-solarized.git  
cd gnome-terminal-colors-solarized  
./install.sh  
cd && rm -rf gnome-terminal-colors-solarized  

# Set up ZSH syntax highlighting
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting  
cd && rm -rf zsh-syntax-highlighting

# Install JetBrains Mono font
https://www.jetbrains.com/lp/mono/

# Set up Vundle
git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim  
vim +PluginInstall +qall   


# Set up SSH to mithril
sudo apt install openssh-server  
sudo systemctl status ssh  
sudo ufw allow ssh  

> May need to remove mithril IP address from ~/.ssh/known_hosts  

cat .ssh/id_rsa.pub | ssh b@B 'cat >> .ssh/authorized_keys'  


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
sh Anaconda3...

conda config --add channels conda-forge   
conda config --add channels anaconda  
conda create --name py37 python=3.7.6
conda install scipy numpy pandas matplotlib scikit-learn tensorflow-gpu beautifulsoup4 

# Install VirtualBox
https://www.virtualbox.org/wiki/Downloads


# Install Dropbox
https://www.dropbox.com/install-linux  
> Waiting for the app to ask for the link code never works. Instead, log in at dropbox.com/login.
```

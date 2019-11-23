---
layout: post
title: Linux setup checklist
---

```
https://www.google.com/chrome/  
  
https://ulauncher.io  
  
https://anydesk.com/en  
  
sudo vim /etc/default/grub  
  
sudo apt-get install vim  
  
sudo apt-get install git  
  
sudo apt-get install curl   
  
sudo apt-get install zsh  
  
https://github.com/ohmyzsh/ohmyzsh#basic-installation  
  
sudo reboot  
  
sudo apt-get install fonts-powerline   
  
mkdir ~/repos   
  
git clone https://github.com/erikr/dotfiles.git  
  
sh create_symlinks.sh  
  
git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim  
  
vim +PluginInstall +qall   
  
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting  
cd && rm -rf zsh-syntax-highlighting

git clone https://github.com/aruhier/gnome-terminal-colors-solarized.git  
cd gnome-terminal-colors-solarized
./install.sh
cd && rm -rf gnome-terminal-colors-solarized
```

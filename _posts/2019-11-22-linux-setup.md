---
layout: post
title: Linux setup guide
---
Here are commands to help set up a new Ubuntu machine on GCP or any cloud platform.

## Install basic utilities
```bash
sudo apt update
sudo apt-get install vim git curl zsh tmux tree unzip make
sudo apt-get upgrade
sudo apt-get dist-upgrade
```

## Set password
```bash
sudo passwd
```

Log out, then back in.

## Shell, dotfiles, zsh theme

Set up Oh-My-ZSH
```bash
sh -c "$(wget -O- https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
sudo chsh -s $(which zsh) $USER
```

Dotfiles and shell scripts
```bash
git clone https://github.com/erikr/dotfiles.git  
cd dotfiles && bash generate-symlinks.sh make
```

zsh-autosuggestions
```bash
git clone https://github.com/zsh-users/zsh-autosuggestions \
 ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
```

Pure theme
```bash
mkdir -p $HOME/.zsh
git clone https://github.com/sindresorhus/pure.git \
 $HOME/.zsh/pure
```

ZSH syntax highlighting
```bash
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git \
 ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
rm -rf zsh-syntax-highlighting
```

Tmux plugin manager (`tpm`)
```
git clone https://github.com/tmux-plugins/tpm \
 ~/.tmux/plugins/tpm
```

## Vim plugins

[vim-plug](https://github.com/junegunn/vim-plug/wiki/tutorial)
```
curl -fLo ~/.vim/autoload/plug.vim --create-dirs \
    https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
```

Install plugins specified in `.vimrc`:
```bash
vim -c PlugInstall
```

## Miniconda and environments

Download and install
```bash
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh 
bash Miniconda3-latest-Linux-x86_64.sh
source ~/miniconda3/bin/activate
```

No need to run `conda init zsh` because my `.zshrc` sets paths.

Delete the install shell script when you finish:

```zsh
rm -rf ~/Miniconda3-latest-Linux-x86_64.sh
```

Set up the Conda environment:
```zsh
conda env create -f ~/dotfiles/environment.yml
```

## Speedtest CLI
```zsh
curl -s https://install.speedtest.net/app/cli/install.deb.sh | sudo bash
sudo apt-get install speedtest
```

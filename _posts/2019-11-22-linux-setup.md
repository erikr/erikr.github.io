---
layout: post
title: Linux setup guide
---
Here are commands to help set up a new Ubuntu machine on GCP or any cloud platform.

## Install basic utilities
```bash
sudo apt update
sudo apt-get install vim git curl zsh tmux tree
```

## Shell, dotfiles, zsh theme

ZSH
```bash
zsh
chsh -s $(which zsh)
sh -c "$(wget -O- https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
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
cd && wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh 
sh Miniconda3
source ~/miniconda3/bin/activate
```

No need to run `conda init zsh` because my `.zshrc` sets paths.

Delete the install shell script when you finish:

```zsh
rm -rf ~/Miniconda3-latest-Linux-x86_64.sh
```

## `gpg`
```bash
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

```bash
PrintLastLog yes
```

and change to

```bash
PrintLastLog no
```
or add if it doesnt exist.

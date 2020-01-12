---
layout: post
title: Setting up data science stack on ERISOne
---

ERISOne is a powerful Linux cluster at Partners. Full specs are described [here](https://rc.partners.org/research-apps-and-services/boilerplates-templates/it-infrastructure#citing-eris).

This post describes how to set up a basic data science stack on ERISOne. The major components are Conda, Tensorflow, and other usual Python packages.

Follow the [setup instructions](https://rc.partners.org/kb/article/2814), connect via SSH with X11 forwarding, and get to a login node.

Change bash to zsh:
```
chsh -s $(which zsh)
```

Log out and back in.  

Set up oh-my-zsh:
```
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

Set up dotfiles:
```
cd
mkdir repos
cd repos
https://github.com/erikr/dotfiles
cd dotfiles
sh create_symlinks.sh
```

Set up Vundle (I have plugins specified in my `~/.vimrc`, which are symlinked in prior step):
```
git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim
vim +PluginInstall +qall
```

Load Anaconda as a "module". Although the latest release is 4.8.1, only 4.3.30 is available on ERISOne.
> Note: Since this and other scripts must be manually called every time you log in to ERISOne, I put it in my [.zshrc](https://github.com/erikr/dotfiles/blob/master/.zshrc) to automatically run at login.

```
module load anaconda
```

Create your Conda environment. I recommend Python 3.7.6, which is the latest 3.7 version, because Tensorflow is not yet compatible with Python 3.8.

```
conda create --name py37_erisone python=3.7.6 scipy numpy pandas matplotlib scikit-learn tensorflow-gpu pytables  
source activate py37_erisone
```

Next, request a session on a compute node with a particular amount of memory and CPU threads. More documentation [here](https://rc.partners.org/kb/article/2680).  

Use 'bsub' (with the '-XF' parameter for X11 forwarding) to start the session with the desired resource reservation (e.g. for 16000MB (16GB) RAM and 10 CPU threads):

```
bsub -Is -XF -R 'rusage[mem=64000]' -n 10 /bin/bash
```























































































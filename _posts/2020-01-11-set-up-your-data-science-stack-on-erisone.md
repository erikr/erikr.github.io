---
layout: post
title: Set up your data science stack on ERISOne
---

ERISOne is a powerful Linux cluster at Partners. Full specs are described [here](https://rc.partners.org/research-apps-and-services/boilerplates-templates/it-infrastructure#citing-eris).

This post describes how to set up a basic data science stack on ERISOne. The major components are Conda, Tensorflow, and other usual Python packages.

Follow the [setup instructions](https://rc.partners.org/kb/article/2814), connect via SSH with X11 forwarding, and get to a login node.

ERISOne only has an old version of zsh (version 4.3). Install a fresher zsh on ERISOne without root by following a slightly modified set of [these instructions](https://franklingu.github.io/programming/2016/05/24/setup-oh-my-zsh-on-ubuntu-without-sudo/). Download and install the latest version of zsh that comes in a `tar.gz`:
```
wget http://www.zsh.org/pub/old/zsh-5.5.1.tar.gz
tar -xzf zsh-5.5.1.tar.gz
rm zsh-5.5.1.tar.gz
cd zsh-5.5.1
mkdir ~/local
./configure --prefix=$HOME/local
make
make check
make install
rm -rf zsh-5.5.1
```

This installs a newer zsh at `~/local/bin/zsh`.

Now, modify `.bashrc` with the path to your freshly installed zsh, and instructions to run it when it starts the bash shell:
```
echo "export PATH=$HOME/local/bin:$PATH" >> ~/.bash_profile
echo "exec zsh" >> ~/.bash_profile
echo "exec zsh" >> ~/.bashrc # seems odd but necessary for later
```

Now, whenever you log in to ERISOne, the bash shell is loaded. This runs `~/.bash_profile`, which in turn 1) modifies the path to include where our source-installed zsh, and 2) loads that fresher zsh shell. Circuitous, but it works.

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
> Note: This and other scripts must be manually called every time you log in to ERISOne, so I put them in my [.zshrc](https://github.com/erikr/dotfiles/blob/master/.zshrc) to automatically run at login.

```
module load anaconda
```

Create your Conda environment. I recommend Python 3.7.6, which is the latest 3.7 version, because Tensorflow is not yet compatible with Python 3.8.

```
conda create --name py37_erisone python=3.7.6 scipy numpy pandas matplotlib scikit-learn tensorflow-gpu pytables  
source activate py37_erisone
```

Next, request a session on a compute node with a particular amount of memory and CPU threads. More documentation [here](https://rc.partners.org/kb/article/2680).  

Use 'bsub' (with the '-XF' parameter for X11 forwarding) to start the session with the desired resource reservation (e.g. for 8000MB (8GB) RAM and 2 CPU threads):

Again, note we are calling the bash shell, but per above our `~/.bash_profile` loads our source-installed zsh shell.
```
bsub -Is -XF -R 'rusage[mem=8000]' -n 2 ~/local/
```

This loads bash, but calls `~/.bashrc` instead of `~/.bash_profile`, hence our earlier modification of `~/.bashrc`.

Now, install vim:

---
layout: post
title: Setting up ERISOne
---

ERISOne is a "computing cluster with a job scheduling system for batch jobs, remote desktops for graphical applications and GPUs running on a Linux OS" maintained by Partners Healthcare.

Ideally, users do not need to purchase and maintain workstations. Rather, they use inexpensive machines (i.e. laptops) to connect to a cluster. Centralizing workflow on a cluster behind the Partners firewall is also ideal for working with PHI.

Unfortunately, setting up a data science stack on ERISOne is challenging due to old software, lack of root access, and complexity in obtaining needed resources. There is a vague limit on cores one can request in a compute node, and we still do not know how to use GPUs.

This guide walks you through setup of 1) Anaconda, Python, and Tensorflow, 2) zsh, and 3) tmux. It assumes you requested and have an ERISOne account, and connect via SSH ([see RISC guide here](https://rc.partners.org/kb/article/2814)).

> Note: I recommend you follow the steps in order. For example, you should install git before oh-my-zsh because the commands for oh-my-zsh assume you have a version of git that is newer than what is available on ERISOne.

## Install Zsh shell

ERISOne comes with Zsh 4.3.11, but the current version is 5.8. Unfortunately since users lack root access on ERISOne, zsh cannot be installed via `yum`.

> Note: you may need to first install `ncurses` from source.

This means we have to install Zsh from source:

```bash
(base) -bash-4.1$ curl -L https://downloads.sourceforge.net/project/zsh/zsh/5.8/zsh-5.8.tar.xz > zsh.tar.xz
(base) -bash-4.1$ tar xf zsh.tar.xz
(base) -bash-4.1$ cd zsh
(base) -bash-4.1$ ./configure --prefix="$HOME/local" \
CPPFLAGS="-I$HOME/local/include" \
LDFLAGS="-L$HOME/local/lib"
(base) -bash-4.1$ make -j && make install
```

Now that Zsh is installed, let's change our default logging shell. However, users on ERISOne do not have root, and thus cannot use `chsh`.

To set Zsh as the default shell, add the following to `.bash_profile`:

```
export PATH=$HOME/local/bin:$PATH
export SHELL=`which zsh`
[ -f "$SHELL" ] && exec "$SHELL" -l
```

`source ~.bash_profile` to make the changes take effect. Now `zsh` will be activated each time you log in to ERISOne.

The next time you log in, you will be prompted to set up the `zsh` shell. The prompt should look different:

```zsh
[ab123@eris1n3]~% 
```

Delete Zsh source files from your home directory:
```zsh
[ab123@eris1n3]~% rm -rf ~/zsh*
```


## Load newer version of git via `module`

ERISOne has git 1.7.1 by default, which was released in 2010. Subsequent installation of oh-my-zsh involves a shell script that invokes `-c` in `git clone`, which was introduced in git 1.7.7. Thus, we must upgrade git.

You can install git from source, but there are many other dependencies, so doing this would take a lot of effort.

Instead we can get a sufficiently recent (albeit not most current) version of git using `module`:

```zsh
module load git/2.17.0
```

> Note: check if a more recent version is available via `module avail git`.


## Install oh-my-zsh

> Oh My Zsh is a delightful, open source, community-driven framework for managing your Zsh configuration. It comes bundled with thousands of helpful functions, helpers, plugins, themes, and a few things that make you shout... "Oh My ZSH!" [https://ohmyz.sh](https://ohmyz.sh)

```zsh
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

## Set up `.zshrc` and other dotfiles

Here is my [`.zshrc`](https://github.com/erikr/dotfiles/blob/master/.zshrc) for reference. When I set up a new machine, I clone my [dotfiles repo](https://github.com/erikr/dotfiles) and run a script to symlink my dotfiles:

```zsh
mkdir repos
cd repos
git clone git@github.com:erikr/dotfiles.git
cd dotfiles
sh generate_symlinks.sh
```

I use the [typewritten](https://github.com/reobin/typewritten) zsh theme. To set up, clone the repository into your custom oh-my-zsh themes directory:

```zsh
git clone https://github.com/reobin/typewritten.git $ZSH_CUSTOM/themes/typewritten
```

Symlink `typewritten.zsh-theme` to your oh-my-zsh custom themes directory:

```zsh
ln -s "$ZSH_CUSTOM/themes/typewritten/typewritten.zsh-theme" "$ZSH_CUSTOM/themes/typewritten.zsh-theme"
```

Set up oh-my-zsh plugin `zsh-syntax-highlighting`:

```zsh
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting```
```

Set up oh-my-zsh plugin `zsh-autosuggestions`:
```
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
```

I use [`vundle`](https://github.com/VundleVim/Vundle.vim) to manage my vim plugins:

```zsh
git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim
```

Install the plugins specified in your `.vimrc` file via:
```zsh
vim +PluginInstall +qall
```

If you log out and back in, your shell should look much improved:
![](/assets/zsh_erisone.png)

## Install Anaconda

The newest version of Anaconda on ERISOne (via `module`) is `4.4.0-p3`, whereas the current Linux version is `4.8.2`. Ideally, one would use the existing Anaconda module on ERISOne and set up a new environment with the desired Python version and packages. Unfortunately, there is an error with older versions of Conda that prevents users from even creating new environments:

```
RemoveError: 'setuptools' is a dependency of conda and cannot be removed from conda's operating environment.
```

To set up the desired Python environment, install Anaconda:

Download the [latest Linux installer for Anaconda with Python 3.x](https://www.anaconda.com/distribution/#download-section):

```zsh
wget https://repo.anaconda.com/archive/Anaconda3-2020.02-Linux-x86_64.sh
bash Anaconda3-2020.02-Linux-x86_64.sh
```

When you get to the license terms, hit `q` to jump all the way to the end where you can type `yes` to accept.

Install Anaconda in the default location, `/PHShome/ab123/anaconda3`, where `ab123` is your Partners username and ERISOne home directory name.

Like most Linux machines, ERISOne uses the bash shell by default. Therefore Conda will modify `/.bashrc` with the appropriate path for you to call `conda`. However, only `/.bash_profile` is called when you log in to ERISOne.

To ensure Conda is on your path so you can call `conda`, source `/.bashrc` in `/.bash_profile`:

```bash
echo 'export source ~/.bashrc' >> ~/.bash_profile
```

After you install Anaconda, log out, and log back in ERISOne, your command line prompt should reflect that you are now using the base environment:

```bash
(base) -bash-4.1$ 
```
Don't forget to delete the Anaconda installer.

## Set up Conda environment

You can set up your environment from a `environment.yml` file. Mine is in my `dotfiles` repo which I clone to my home directory.
```zsh
conda env create -f ~/dotfiles/environment.yml
```


> Note: I use Python 3.7 because Tensorflow 2 does not yet support Python 3.8.

You can also create an environment using commands instead of an `environment.yml`. Read the [conda documentation](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html) to learn more.

After a long installation process, activate the environment. Your prompt should change to reflect the activated environment name:

```zsh
(base) -bash-4.1$ conda activate py37
(py37) -bash-4.1$ 
```

## tmux

A battle for another day.


## Interactive session on compute node

Users are told to request a compute node instead of running jobs on the login nodes (`eris1n2` or `eris1n3`):

```zsh
bsub -XF -R 'rusage[mem=10000]' -n 8 -Is /bin/bash
```

This requests an interactive bash session with 10 GB RAM and 8 CPU cores. However, if you use my dotfiles, `.bash_profile` will launch zsh.

Read more about compute nodes on ERISOne [here](https://rc.partners.org/kb/article/2680).


## Mount ERISOne as a network drive in Linux
First, create the directory on your local machine in which to mount your ERISOne home directory:

```
$ USER="ab123"
$ MOUNTDIR="/mnt/$user"
$ sudo mdkir $MOUNTDIR
```

This creates an empty directory in `/mnt/ab123`.

> If you use `zsh` you may need to do `sudo \mkdir $MOUNTDIR`.


You have two options to mount:

1. `SSHFS`
```
sudo sshfs -o allow_other $USER@erisone.partners.org:/PHShome/$USER $MOUNTDIR
```

2. `cifs-util`

```
sudo mount -t cifs -o username=$USER,domain=partners,vers=1.0,uid=1000 //eris1fs2.partners.org/$USER /$MOUNTDIR
```

You will be prompted to enter your password.

If successful, you won't get any errors. You can then navigate to the local mount location and access your home directory on ERISOne:

```
$ cd $MOUNTDIR
$ pwd
/mnt/ab123
$ erik@localmachine > ls -l
total 1024
drwxrwx---+ 5 erik 10064  0 Jan 12 01:07 local
drwxrwxr-x+ 3 erik 10064  0 Nov  3  2017 lsf
drwxrwx---+ 2 erik 10064  0 Jul 29 09:00 perl5
drwxrwx---+ 4 erik 10064  0 Jan 12 10:10 repos
lrwxrwxrwx  1 erik root  16 Jul 26  2019 scratch -> /scratch/e/ab123
```
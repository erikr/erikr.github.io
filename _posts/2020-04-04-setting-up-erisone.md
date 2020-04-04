---
layout: post
title: Setting up ERISOne
---

ERISOne is a "computing cluster with a job scheduling system for batch jobs, remote desktops for graphical applications and GPUs running on a Linux OS" maintained by Partners Healthcare.

Ideally, users do not need to purchase and maintain workstations. Rather, they use inexpensive machines (i.e. laptops) to connect to a cluster. Centralizing workflow on a cluster behind the Partners firewall is also ideal for working with PHI.

Unfortunately, setting up a data science stack on ERISOne is challenging due to old software, lack of root access, and complexity in obtaining needed resources. There is a vague limit on cores one can request in a compute node, and we still do not know how to use GPUs.

This guide walks you through setup of 1) Anaconda, Python, and Tensorflow, 2) zsh, and 3) tmux. It assumes you requested and have an ERISOne account, and connect via SSH ([see RISC guide here](https://rc.partners.org/kb/article/2814)).

> Note: I recommend you follow the steps in order. For example, you should install git before oh-my-zsh because the commands for oh-my-zsh assume you have a version of git that is newer than what is available on ERISOne.

## Install Anaconda

The newest version of Anaconda on ERISOne (via `module`) is `4.4.0-p3`, whereas the current Linux version is `4.8.2`. Ideally, one would use the existing Anaconda module on ERISOne and set up a new environment with the desired Python version and packages. Unfortunately, there is an error with older versions of Conda that prevents users from even creating new environments:

```
RemoveError: 'setuptools' is a dependency of conda and cannot be removed from conda's operating environment.
```

To set up our desired Python environment, a user must therefore install their own version of Anaconda:

Download the [latest Linux installer for Anaconda with Python 3.x](https://www.anaconda.com/distribution/#download-section)

```bash
$ cd && wget url_from_above
$ bash installer_name
```

When you get to the license terms, hit `q` to jump all the way to the end where you can type `yes` to accept.

Install Anaconda in the default location, `/PHShome/ab123/anaconda3`, where `ab123` is your Partners username and ERISOne home directory name.

Like most Linux machines, ERISOne uses the bash shell by default. Therefore Conda will modify `/.bashrc` with the appropriate path for you to call `conda`. However, only `/.bash_profile` is called when you log in to ERISOne.

To ensure Conda is on your path so you can call `conda`, source `/.bashrc` in `/.bash_profile`:

```bash
$ echo 'export source ~/.bashrc' >> ~/.bash_profile
```

After you install Anaconda, log out, and log back in ERISOne, your command line prompt should reflect that you are now using the base environment:

```bash
(base) -bash-4.1$ 
```
> Note: for brevity I won't write every code block with my full prompt, just a `$` or `%` to indicate we are at the shell.

Don't forget to delete the 693M Anaconda installer.

## Set up Conda environment

You can set up your environment from a `environment.yml` file:
```bash
$ wget https://raw.githubusercontent.com/erikr/dotfiles/master/py37.yml
$ conda env create -f py37.yml

```

> Note: I use Python 3.7.x because Tensorflow 2 does not yet support Python 3.8.

You can also create an environment using commands instead of an `environment.yml`. Read the [conda documentation](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html) to learn more.

After a long installation process, activate the environment. Your prompt should change to reflect the activated environment name:

```bash
(base) -bash-4.1$ conda activate py37
(py37) -bash-4.1$
```

You can delete `py37.yml`.


## Install Zsh shell

ERISOne has an outdated version of Zsh installed:

```bash
$ which zsh
/bin/zsh
$ /bin/zsh --version
zsh 4.3.11 (x86_64-redhat-linux-gnu)
```

This older version lacks some features we want. The current Zsh version is 5.8. Unfortunately since users lack root access on ERISOne, zsh cannot be installed via `yum`.

> Note: you may need to first install ncurses from source.

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

Call `source ~.bash_profile` to make the changes take effect. Now, `zsh` will be activated each time you log in to ERISOne.

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
% git --version
git version 1.7.1
% module load git/2.17.0
% git --version
git version 2.17.0
```

> Note: check if a more recent version is available by calling `module avail git`.


## Install oh-my-zsh

> Oh My Zsh is a delightful, open source, community-driven framework for managing your Zsh configuration. It comes bundled with thousands of helpful functions, helpers, plugins, themes, and a few things that make you shout... "Oh My ZSH!" [https://ohmyz.sh](https://ohmyz.sh)

```zsh
% sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

## Set up `.zshrc` and other dotfiles

Here is my [`.zshrc`](https://github.com/erikr/dotfiles/blob/master/.zshrc) for reference. I clone my entire dotfiles repo and run a script to symlink all of my dotfiles.

```zsh
% mkdir repos
% cd repos
% git clone git@github.com:erikr/dotfiles.git
% cd dotfiles
% sh generate_symlinks.sh
```

My `.zshrc` set the theme to [typewritten](https://github.com/reobin/typewritten), which is my current favorite theme. Here's how to install it.

Clone the repository into your custom oh-my-zsh themes directory:

```zsh
% git clone https://github.com/reobin/typewritten.git $ZSH_CUSTOM/themes/typewritten

```

Symlink `typewritten.zsh-theme` to your oh-my-zsh custom themes directory:

```zsh
% ln -s "$ZSH_CUSTOM/themes/typewritten/typewritten.zsh-theme" "$ZSH_CUSTOM/themes/typewritten.zsh-theme"
```

Set up oh-my-zsh plugins. I like `zsh-syntax-highlighting` and `zsh-autosuggestions`: 

```zsh
% git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
% git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
```

I use [`vundle`](https://github.com/VundleVim/Vundle.vim) to manage my vim plugins. Install it, and the other plugins I've specified in my `.zshrc`:

```zsh
% git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim
% vim +PluginInstall +qall
```

If you log out and back in, your shell should look much improved:
![](/assets/zsh_erisone.png)


## tmux

A battle for another day.


## Interactive session on compute node

You should not run computational jobs on the login nodes (`eris1n2` or `eris1n3`). Instead, request a compute node:

```zsh
> bsub -Is -XF -R 'rusage[mem=16000]' -n 8
```

This requests an interactive session with 16 GB of RAM and 8 CPU cores, and launches Zsh.

Read more about compute nodes on ERISOne [here](https://rc.partners.org/kb/article/2680). Note their default command calls bash, not Zsh.
---
layout: post
title: Setting up ERISOne
---

ERISOne is a "computing cluster with a job scheduling system for batch jobs, remote desktops for graphical applications and GPUs running on a Linux OS" maintained by Partners Healthcare.

Ideally, users do not need to purchase and maintain compute workstations. Rather, we use Macbooks to connect to a cluster behind the Partners firewall, This is ideal for working on a shared data repository, especially since we analyze PHI.

Unfortunately, setting up a modern data science stack on ERISOne is challenging. The default software packages are outdated, and obtaining desired memory, GPU, and CPU resources on a compute node adds friction to workflow.

This guide walks you through setup of Zsh, git, . It assumes you requested and have an ERISOne account, and connect via SSH ([see RISC guide here](https://rc.partners.org/kb/article/2814)).

> Note: I recommend you follow the steps in order. For example, you should install git before oh-my-zsh because the commands for oh-my-zsh require a recent version of git.

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

If you log out and back in, your shell should look much improved:
![](/assets/zsh_erisone.png)

## Load `git`, `tmux`, & `vim` via `module`

ERISOne has git 1.7.1 by default, which was released in 2010. Subsequent installation of oh-my-zsh involves a shell script that invokes `-c` in `git clone`, which was introduced in git 1.7.7. Thus, we must upgrade git.

You can install git from source, but there are many other dependencies, so doing this would take a lot of effort.

Instead we can get a sufficiently recent (albeit not most current) version of git using the `load` command of `module`:

```zsh
module load git/2.17.0
```

Check if a more recent version of `MODULE_NAME` is available via `module avail MODULE_NAME`.

My `.zshrc` loads newer versions of `git`, `tmux`, and `vim`.

## Install Anaconda

The newest version of Anaconda on ERISOne (via `module`) is `4.4.0-p3`, whereas the current Linux version is `4.8.3`. Ideally, one would use the existing Anaconda module on ERISOne and set up a new environment with the desired Python version and packages. Unfortunately, there is an error with older versions of Conda that prevents users from even creating new environments:

```
RemoveError: 'setuptools' is a dependency of conda and cannot be removed from conda's operating environment.
```

To set up the desired Python environment, install Miniconda following the instructions in my [Linux setup post](../linux-setup). 

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

## Interactive session on compute node

Users are told to request a compute node instead of running jobs on the login nodes (`eris1n2` or `eris1n3`):

`mem` specifies Mb of memory, and `-n` specifies number of cores.

```zsh
bsub -Is -XF -R 'rusage[mem=8000]' -n 4 /PHShome/$USERNAME/local/bin/zsh
```

This requests an interactive session with 8 GB RAM and 4 CPU cores.

To get more cores in an interactive session, use the `interact-big` queue:

```zsh
bsub -q interact-big -Is -XF -R 'rusage[mem=16000]' -n 10 /PHShome/$USERNAME/local/bin/zsh
```

Read more about compute nodes on ERISOne [here](https://rc.partners.org/kb/article/2680).


## Mount ERISOne as a network drive
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
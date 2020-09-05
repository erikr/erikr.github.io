---
layout: post
title: Set up Miniconda on Aguirre or Stultz Lab machine 
---

Here are instructions for a user of an Aguirre or Stultz Lab machine to set up Miniconda.

Miniconda is installed in the administrator account. You should access and use this installation, and can modify it and install packages.

For Aguirre Lab, this account name is `aguirrelab`. For Stultz Lab, this account name is `stultzlab`.

In the below examples I will use `aguirrelab` in paths. If you are working wiht a Stultz Lab machine, use `stultzlab` instead.

Activate:

```zsh
$ source /home/aguirrelab/miniconda3/bin/activate
```

Your console prompt should now be prepended by `(base)`.

Create an environment from your `environment.yml` config file. My `dotfiles` repo contains an example [`environment.yml`](https://github.com/erikr/dotfiles/blob/master/environment.yml) named `er`. It contains Python 3.8, Numpy, Pandas, h5py, Jupyter Lab, and other packages I use across projects. You are free to copy and use it as helpful.

Project repos may contain an `environment.yml`, or you can [create an environment with commands](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html#creating-an-environment-with-commands).

```zsh
$ conda env create -f environment.yml
$ conda activate er
```

Your prompt should now be prepended by the environment name; if you use my `environment.yml`, the prompt is now prepended by `(er)`.

Update your `zsh` shell config `.zshrc` so new shell instances have the path to Conda:

```zsh
$ conda init zsh
```

Check to ensure your path has the miniconda environment version of Python, and that the version is correct:

```zsh
$ which python
/home/aguirrelab/miniconda3/envs/er/bin/python
  
$ python --version
Python 3.8.5
```
---
layout: post
title: Set up Miniconda on Aguirre Lab machine 
---

Here are instructions for a user of an Aguirre Lab machine to set up Miniconda.

Miniconda is installed in the administrator account, `aguirrelab`. You should access and use this installation, and can modify it and install packages.

Activate:

```zsh
$ source /home/aguirrelab/miniconda3/bin/activate
```

Your console prompt should now be prepended by `(base)`.

Create an environment from your `environment.yml`. My usual environment is in my `dotfiles` repo. It is called `er` and it contains Python 3.8, Tensorflow, Numpy, Pandas, h5py, etc. Your repo may have its own `environment.yml`, or you can [create an environment with commands](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html#creating-an-environment-with-commands).

```zsh
$ conda env create -f environment.yml
$ conda activate er
```

Your prompt should now be prepended by `(er)` (or whatever your environment name is).

If you use my `.zshrc`, every time a new shell is opened, the script `conda.sh` is called, and if you are in a Tmux session, conda is re-activated.

If you do not use my `.zshrc` or something similar, update your `.zshrc` so new shells know the path to Conda:

```zsh
$ conda init zsh
```

Check to ensure your path has the miniconda environment version of Python, and that the version is correct:

```zsh
$ which python
/home/aguirrelab/miniconda3/envs/er/bin/python
  
$ python --version
Python 3.8.3
```
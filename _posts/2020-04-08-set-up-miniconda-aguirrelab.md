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

Create an environment from your `environment.yml`. My usual environment is in my `dotfiles` repo. It is called `py37` and it contains Python 3.7.7 (Tensorflow 2 does not yet support Python 3.8) and a variety of popular packages such as Tensorflow, Numpy, Pandas, h5py, etc. Your repo may have its own `environment.yml`, or you can [create an environment with commands](https://docs.conda.io/projects/conda/en/latest/user-guide/tasks/manage-environments.html#creating-an-environment-with-commands).

```zsh
$ conda env create -f environment.yml
$ conda activate py37
```

Your prompt should now be prepended by `(py37)` (or whatever your environment name is).

If you use my `.zshrc`, you are done!

If not, update your `.zshrc` so when you launch a new shell it knows the paths to conda:

```zsh
$ conda init zsh
```

Append to the end of your `~/.zshrc` the following to always activate the `py37`environment when you log in:

```
CONDA_CUSTOM_ENV="py37"
conda deactivate; conda activate $CONDA_CUSTOM_ENV
```

The deactivation following by reactivation seems redundant but it is required to avoid problems with `tmux`.

Check to ensure your path has the miniconda environment version of Python, and that the version is correct:

```zsh
$ which python
/home/aguirrelab/miniconda3/envs/py37/bin/python
  
$ python --version
Python 3.7.7
```
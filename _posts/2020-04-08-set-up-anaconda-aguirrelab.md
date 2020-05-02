---
layout: post
title: Set up Anaconda on Aguirre Lab machine 
---

Here are instructions for a user of an Aguirre Lab machine to set up Anaconda.

Anaconda is installed in the administrator account, `aguirrelab`. You should access and use this installation, and can modify it and install packages.

Activate:

```zsh
> source /home/aguirrelab/anaconda3/bin/activate
(base) >
```

Your console prompt should now be prepended by `(base)`.

Next, activate the environment `py37`, which contains Python 3.7.7 (Tensorflow 2 does not yet support Python 3.8) and a variety of popular packages such as Tensorflow, Numpy, Pandas, h5py, etc.:

```zsh
(base) > conda activate py37
(py37) >
```

Your prompt should now be prepended by `(py37)`.

Update your shell dotfiles so when you launch a new shell it knows the paths to conda:

> Note: if you use my `.zshrc`, skip this step.

```zsh
(py37) > conda init zsh
```

Lastly, append to the end of your `~/.zshrc` the following to always activate the `py37`environment when you log in:

```
CONDA_CUSTOM_ENV="py37"
conda deactivate; conda activate $CONDA_CUSTOM_ENV
```

The deactivation following by reactivation seems redundant but it is required to avoid problems with `tmux`.

Check to ensure your path has the Anaconda environment version of Python, and that the version is correct:

```zsh
(py37) > which python
/home/aguirrelab/anaconda3/envs/py37/bin/python
(py37) > python --version
Python 3.7.7
```
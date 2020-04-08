---
layout: post
title: Set up Anaconda with multiple users
---

These instructions are specific to Mithril and may not work elsewhere.

Activate Conda, which is installed in the `aguirrelab` admin account home directory:

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

Next, let Conda update your dotfile with the correct path by calling `init`:

```zsh
(py37) > conda init zsh
```

Now, the next time you log in, your shell knows where Conda is.

Lastly, append to the end of your `~/.zshrc` the following to always activate the `py37`environment when you log in:

```
CONDA_CUSTOM_ENV="py37"
conda deactivate; conda activate $CONDA_CUSTOM_ENV
```

The deactivation following by reactivation seems redundant but it is required to avoid some problems with tmux.

Check to ensure your path has the Anaconda environment version of Python, and that the version is correct:

```zsh
(py37) > which python
/home/aguirrelab/anaconda3/envs/py37/bin/python
(py37) > python --version
Python 3.7.7
```
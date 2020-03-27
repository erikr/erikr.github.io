---
layout: post
title: Setting up a Jupyter Lab remote server
---

# Intro

# 1. Password setup

Log in to the remote server:

```
$ jupyter notebook password
Enter password:  ****
Verify password: ****
[NotebookPasswordApp] Wrote hashed password to /Users/you/.jupyter/jupyter_notebook_config.json
```

# 2. Add aliases to your `~/.aliases` (or equivalent)

```
remote_user="your_name"
remote_machine="server_address"
PORT=1234
alias remote_notebook_start='ssh -f ${remote_user}@${remote_machine} ". ~/.zshrc; jupyter-lab --no-browser --port=${PORT}"'
alias remote_notebook_stop='ssh ${remote_user}@${remote_machine} "pkill -u ${remote_user} jupyter"'
alias port_forward='ssh -N -f -L localhost:${PORT}:localhost:${PORT} ${remote_user}@${remote_machine}; open http://localhost:${PORT} &'
```

My `.aliases` dotfile is in my [dotfiles repo](https://github.com/erikr/dotfiles/blob/master/.aliases).

> Note: non-interactive SSH login does not source your dotfiles, so the path to your Jupyter installation is not set. To address, need to source `.zshrc` (or whichever dotfile you use to set your paths).

# 3. Source your `~/.aliases` so you can use the aliases
```
source ~/.aliases
```

# 4. Start JupyterLab on the remote server from your local machine

```
$ remote_notebook_stop
$ [I 12:57:27.298 LabApp] JupyterLab extension loaded from /home/er498/anaconda3/envs/py37/lib/python3.7/site-packages/jupyterlab
[I 12:57:27.298 LabApp] JupyterLab application directory is /home/er498/anaconda3/envs/py37/share/jupyter/lab
[I 12:57:27.299 LabApp] Serving notebooks from local directory: /home/er498
[I 12:57:27.299 LabApp] The Jupyter Notebook is running at:
[I 12:57:27.299 LabApp] http://localhost:3745/
[I 12:57:27.299 LabApp] Use Control-C to stop this server and shut down all kernels (twice to skip confirmation).
```

> Hit "return" after you see above output to return to your console.

# 5. Forward port and open notebook from your local machine
```
$ port_forward
bind [127.0.0.1]:3745: Address already in use
channel_setup_fwd_listener_tcpip: cannot listen to port: 3745
Could not request local forwarding.
[1] 48429
[1]  + 48429 done       open http://localhost:${PORT}
$ [I 12:58:28.234 LabApp] 302 GET / (127.0.0.1) 1.15ms     dotfiles -> master
[W 12:58:29.073 LabApp] Could not determine jupyterlab build status without nodejs
```

The `port_forward` alias opens your default browser and navigates to the notebook:

![](/assets/jupyter-screenshot.png)

# Other resources
https://ljvmiranda921.github.io/notebook/2018/01/31/running-a-jupyter-notebook/  
https://towardsdatascience.com/running-jupyter-notebooks-on-remote-servers-603fbcc256b3  
https://www.blopig.com/blog/2018/03/running-jupyter-notebook-on-a-remote-server-via-ssh  
https://agent-jay.github.io/2018/03/jupyterserver/
# Kaibo's Aliases
Here are some aliases that I use in my daily work. They are defined in my `.bashrc` file. This is a work in progress. I will update this file as I add more aliases.

## For monitoring CPU and GPU usage
Since my work tends to be memory-intensive, I use `top -o RES` to monitor memory usage. Default refresh interval is 3 seconds. Here, I set it to 1 second because 3 seconds is too slow for me. Note that `top` also allows you to monitor load average and CPU usage, which I find also useful.

```bash
alias t='top -o RES -d 1'
```

For monitoring GPU usage, I use `nvidia-smi`. I update it every second.

```bash
alias n='watch -n 1 nvidia-smi'
```

## For working with MATLAB
I work with MATLAB and Python a lot. One [common issue](https://www.mathworks.com/matlabcentral/answers/329796-issue-with-libstdc-so-6) is the `GLIBCXX_3.4.21` error (iykyk). A workaround is to use `LD_PRELOAD` to load the correct version of `libstdc++.so.6`, preferably the one in your conda environment, say `py311`. So, I do that in the alias first. Then, I need to activate my conda environment `py311`. Finally, I `cd` to my working directory and start MATLAB.

```bash
m(){
    export LD_PRELOAD=/path/to/miniforge3/envs/py311/lib/libstdc++.so.6
    mamba activate py311
    cd /path/to/my/working/directory
    matlab
}
```

## For working with VNC server
Aside from accessing the Linux workstation through SSH using VSCode, I also use VNC server sometimes, especially when I need to use `fsleyes`. I set the resolution to `2560x1440` because it is the resolution of my laptop screen. 

```bash
alias vnc='vncserver -geometry 2560x1440'
```

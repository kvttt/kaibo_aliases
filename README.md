# Kaibo's Aliases
Here are some aliases that I use in my daily work. They are defined in my `.bashrc` file. This is a work in progress. I will update this file as I add more aliases.

## For checking home directory usage
My home directory is limited to 5 GB. Therefore, once in a while I need to check how much space I have left. `.??*` matches all hidden files/directories with at least two characters (such that `.` and `..` are excluded). `*` matches all non-hidden files/directories. I sort the output by size in human-readable format and in reverse order.

```bash
duh(){
    cd
    du -skh .??* * | sort --human-numeric-sort --reverse
}
```

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

## For running Python scripts
I often need to run Python script overnight. I use `nohup` to prevent the script from being killed when I log out and I use `&` to run the script in the background. In the following alias, the first positional argument `$1` is the Python script that I want to run and the second positional argument `$2` is the output file that I want to save the output to. The `echo $!` command prints the PID of the Python script. Then, I call `top` and use `xargs` to pass the PID to `grep` such that I can easily monitor the CPU and memory usage of the Python script.

```bash
py(){
    nohup python -u $1 > $2 & echo $! | xargs -I {} bash -c 'top -d 1 -b | grep {}'
}
```

## For working with $\LaTeX$

At school and at work, I use $\LaTeX$ a lot. Here are some shorthands that I put in the preamble of my documents.

```latex
\documentclass[12pt, letterpaper, oneside, openany]{article}
\usepackage{amsmath, amsthm, amssymb, amsfonts}

% Theorem environments
\theoremstyle{plain}
\newtheorem{theorem}{Theorem}[section]
\newtheorem{lemma}[theorem]{Lemma}
\newtheorem{corollary}[theorem]{Corollary}
\newtheorem{definition}[theorem]{Definition}
\newtheorem{example}[theorem]{Example}
\newtheorem{proposition}[theorem]{Proposition}
\newtheorem{remark}[theorem]{Remark}

% For calculus
\DeclareMathOperator{\grad}{grad}
\DeclareMathOperator{\curl}{curl}
\DeclareMathOperator{\Div}{div}

% For real, complex, and functional analysis
\DeclareMathOperator{\dist}{dist}
\DeclareMathOperator{\supp}{supp}
\DeclareMathOperator{\re}{Re}
\DeclareMathOperator{\im}{Im}

% For linear algebra
\DeclareMathOperator{\rank}{rank}
\DeclareMathOperator{\tr}{tr}
\DeclareMathOperator{\diag}{diag}

% Misc
\DeclareMathOperator{\sgn}{sgn}
\DeclareMathOperator*{\argmin}{arg\,min}
\DeclareMathOperator*{\argmax}{arg\,max}
```

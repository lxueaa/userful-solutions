# Install Anaconda in csh shell

**1. Download Installer**

Go to [Anaconda](https://www.anaconda.com/distribution/) webpage, select the version you need. Here I choose the lateset Linux Version.

```bash
$ wget https://repo.anaconda.com/archive/Anaconda3-2019.03-Linux-x86_64.sh
```

**2. Install Anaconda**

```bash
$ bash Anaconda3-2019.03-Linux-x86_64.sh
```

During installation, you will need to decide where to install your anaconda. I install it here:

```bash
$ /home/data/lxueaa/anaconda3
```

You need also set $PATH environment in your source file. For me, it is under directory:

```bash
$ ~/.cshrc_user
```

To set the $PATH, add following line to the end of your source file:

```bash
setenv PATH "${PATH}:/home/data/lxueaa/anaconda3/bin"
```

Then close your terminal and restart it. Type:

```bash
conda
```

If you successfully install it, you will see a list of conda manual.

**3. Initialize Anaconda**

To initialize your shell, run this command and restart your terminal:

```bash
$ conda init <SHELL_NAME>
```

To check your shell name (bash, fish, tcsh, xonsh, zsh, or powershell), you can run and see:

```bash
$ echo $0
-tcsh
```

Actually, my shell type is csh. This will cause some trouble when activate your virtual environment. We will introduce one possible solution here, proved work on my situation, to see how to solve it.

**4. Create your own python enviroment**

 To create a new environment (e.g. we name this environment as py373)  just type:

```bash
$ conda create -n py373 python=3.7.3
```

After create it, you can run following to see a list of virtual environment created:

```bash
$ conda info --envs
WARNING: The conda.compat module is deprecated and will be removed in a future release.
# conda environments:
#
base                  *  /home/data/lxueaa/anaconda3
py373                    /home/data/lxueaa/anaconda3/envs/py373
```

To use **py373**, you need to activate it. The star '*' means **base** is currently activated.


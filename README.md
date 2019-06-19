# Install Anaconda in C-shell (csh)

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
$ conda
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

**5. Activate your environment**

According to the Anaconda Docs, to activate **py373**, you need to type:

```bash
$ conda activate py373
```

Unfortunately, since we use csh shell, you will see:

```bash
$ conda activate py373

CommandNotFoundError: Your shell has not been properly configured to use 'conda activate'.
To initialize your shell, run

    $ conda init <SHELL_NAME>

Currently supported shells are:
  - bash
  - fish
  - tcsh
  - xonsh
  - zsh
  - powershell

See 'conda init --help' for more information and options.

IMPORTANT: You may need to close and restart your shell after running 'conda init'.
```

I Googled a lot of solutions, and finally solve it based on [Caoxiang Zhu's Blog](https://zhucaoxiang.github.io/2019/02/20/Use-Conda-Activate-in-C-Shell.html). Here are detailed solutions:

**IMPORTANT: This ONLY works when your local default python version is 2.x. I tried it with local python 3.7.1, it did not works. Not sure for other python 3.x version.**

1. Download the [activate.csh]() and [deactivate.csh]() files in your installation directory. In my case, it is:

   ```bash
   $ \home\data\lxueaa\anaconda3\bin\
   ```

2. Set environment variables. Open the environment file in home directory, in my case is:

   ```bash
   $ vim ~/.cshrc_user
   ```

   Then, add the following to the end of your file:

   ```bash
   setenv CONDA_ENVS_PATH /home/data/lxueaa/anaconda3/envs
   alias activate 'source /home/data/lxueaa/anaconda3/bin/activate.csh'
   alias deactivate 'source /home/data/lxueaa/anaconda3/bin/deactivate.csh'
   ```

   Restart your terminal.

3. Activate **py373** by

   ```bash
   $ activate py373
   basename: extra operand ‘’
   Try 'basename --help' for more information.
   Your Python environment has been changed to the 'py373' conda environment. Here's the active version of Python:
   /home/data/lxueaa/anaconda3/envs/py373/bin/python
   Python 3.7.3
   To switch back to your default Python environment, type 'source deactivate.csh'
   ```

4. Deactivate **py373** by

   ```bash
   $ deactivate py373
   Your Python environment has been reset. Here's the active version of Python:
   /bin/python
   Python 2.7.5
   ```

**6. Conda Install Packages**

When using conda install packages, for example:

```bash
$ conda install mxnet 
```

You may encounter this problem, expecially when you are using a cluster have many users, and each user has limited disk quota in their home directory:

```bash
ERROR conda.core.link:_execute(568): An error occurred while installing package 'None'.
OSError(122, 'Disk quota exceeded')
```

This problem happens because under your home directory, there is a hidden folder named **.conda**. By default, conda will download and cache packages in this folder, and this may cause user disk quota exceeded. 

```bash
$ ls -a
.          .cache    .config      DOS       .login       public_html      .viminfo
..         .conda    .cshrc       .history  .login_user  .python_history  .Xauthority
.anaconda  .condarc  .cshrc_user  .local    perl5        .vim
```

To solve this, we need to change the default cache directory by modify **.condarc** file, usually in home directory. 

```bash
$ vim .condarc
```

Edit **.condarc**: by adding following lines to the file:

```bash
auto_activate_base: true                                                              
envs_dirs:
  - /home/data/lxueaa/anaconda3/envs
pkgs_dirs:
  - /home/data/lxueaa/anaconda3/pkgs
```

Save and restart your terminal. Solved.

**7. Possible problem and solutions**

Because we did something on activate, sometimes when we install packages, it may not install to our current virtual environment. For example, when I install pytorch, I want it is installed under folder */home/data/lxueaa/anaconda3/envs/py373/bin*. However, it is installed in */home/data/lxueaa/anaconda3/bin*.

To solve this, you can directly go to your virtual environment bin and install it by using pip:

```bash
$ cd /home/data/lxueaa/anaconda3/envs/py373/bin
$ pip install https://download.pytorch.org/whl/cu100/torch-1.1.0-cp37-cp37m-linux_x86_64.whl
$ pip install https://download.pytorch.org/whl/cu100/torchvision-0.3.0-cp37-cp37m-linux_x86_64.whl
```


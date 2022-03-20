# Disco Diffusion v5 Turbo, with 3D animation, running locally.
Getting the latest versions of Disco Diffusion ([at time of writing this is v5 with Turbo and 3D Animation](https://colab.research.google.com/github/zippy731/disco-diffusion-turbo/blob/main/Disco_Diffusion_v5_Turbo_%5Bw_3D_animation%5D.ipynb)) to work locally, instead of colab. Including how I run this on Windows, despite some Linux only dependencies ;)

If you run into any issues, feel free to open an issue and I’ll do my best to help troubleshoot. Be as specific as possible. For example, if you get an error message at any point you need to include this in the issue, alongside operating system and computer specs.

## How to run this on Windows
The same steps should work on Linux, starting from step 3.

Requirements:
* Nvidia GPU with at least 8GB VRAM. >12GB is recommended. 
* Windows 10 or 11
### Step 1: Update windows

Windows 11: Windows 11 should work but it doesn't hurt to update to the latest version before continuing :) (note though that I've created and tested this on Windows 10)
Windows 10: You must be running at least feature update 21H2 for your GPU to work. To check which version you're running, open cmd and type: 

    winver
If you’re on 21H2 or later, you’re good to go. If not, try updating Windows by typing “check for updates” in the start menu and using the built in tool. For me personally, the required update was not showing, but I was able to install it by using the [Windows 10 Update Assistant](https://support.microsoft.com/en-us/topic/windows-10-update-assistant-3550dfb2-a015-7765-12ea-fba2ac36fb3f).

### Step 2: install WSL2
The way we’ll use Linux only dependencies is by installing the latest version of the Windows Subsystem for Linux (WSL2). This will run a virtual machine–like Ubuntu installation on Windows. However, Microsoft have implemented this at a very low level, meaning almost no performance hit and GPU support! 

 For the latest instructions on this, follow the [official Microsoft guide](https://docs.microsoft.com/en-us/windows/wsl/install).

Briefly, just open a Windows Powershell as administrator, and type:

    wsl —-install

It might request a restart, and when you restart your computer you’ll have an app in the start menu or task bar called “Ubuntu”!

### Step 3:  install Anaconda on Ubuntu
We’ll need to install Anaconda inside our Ubuntu environment to manage packages easily. Open your new Ubuntu app (and fix any errors that come up on first launch. I had a few, but they were either self explanatory or fixed easily with some quick Googling). 
Now you want to download, then run, the Linux Anaconda installer as follows. If you’re following this much later than March 2022 you can replace the url below with the latest version from [the Anaconda website].(https://www.anaconda.com/products/individual?modal=nucleus)

    mkdir Downloads 
    cd Downloads 
    wget https://repo.anaconda.com/archive/Anaconda3-2021.11-Linux-x86_64.sh
    bash Anaconda3-2021.11-Linux-x86_64.sh

Follow the on-screen instructions. Ie type yes when it asks you to, and ask it to run conda init for you when prompted. 

Close your Ubuntu terminal and open it again. 

Now type and run

    conda —-help
If it gives you a long list of conda options, that means it’s successfully installed Anaconda within Ubuntu!

### Step 4: creating our environment

We’ll now create and activate a conda environment (inside Ubuntu) with all the appropriate dependencies. 

    conda create -n pytorch_110
    conda activate pytorch_110

Whenever you restart your computer, or close and open Ubuntu again, you will have to run that second command (conda activate pytorch). Now install the correct version of pytorch:

    conda install pytorch==1.10 torchvision torchaudio cudatoolkit=11.1 -c pytorch -c conda-forge

Type y whenever prompted. 

Finding the above took a lot of trial and error. The difficulty was finding a pytorch and cudatoolkit combination which works with pytorch3d (required later). The above work for me.

Now install some other dependencies:

    conda install jupyter pandas requests matplotlib
    conda install opencv -c conda-forge

### Step 5: Download the notebook
We’ll be working within a jupyter notebook version of the colab notebook. (I’m currently working on a cleaner interface, make sure to star and watch the repo to see when this goes live). 

Download the jupyter notebook in this repo. If you know how, clone the repo directly into your Ubuntu distribution. To make this guide as easy to follow as possible, I’ll also show an easier way. 

In your Ubuntu terminal, type:

    explorer.exe .

This will open your Ubuntu directory in Windows Explorer! Find a location you want to download the notebook to, maybe create a new folder for it.

On my github repository, click “code” then download zip. Extract the zip, and copy the .ipynb file to the desired folder **in Ubuntu**. If typed explorer.exe earlier, you’ll have an Ubuntu folder open in Explorer, so you can drag and drop into this folder. 

### Step 6: Run the notebook!

In your Ubuntu terminal, run:

    jupyter notebook

You might notice that this doesn’t automatically open jupyter in your browser. That’s okay! Just look for the URL starting with localhost, copy this, and paste it into your browser on Windows.

This should open jupyter in your browser! Now navigate to the folder where you have placed your jupyter notebook, and open it. Run the cells, one by one. They should install further required dependencies and download all the models for you. Along the way, you can change any settings you would like. One of the last cells asks for “text_prompts”, which you can specify to create whatever you wish!

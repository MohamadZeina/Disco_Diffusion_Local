# Disco Diffusion v5 Turbo, with 3D animation, running locally.
Getting the latest versions of Disco Diffusion ([at time of writing this is v5 with Turbo and 3D Animation](https://colab.research.google.com/github/zippy731/disco-diffusion-turbo/blob/main/Disco_Diffusion_v5_Turbo_%5Bw_3D_animation%5D.ipynb)) to work locally, instead of colab. Including how I run this on Windows, despite some Linux only dependencies ;). Now includes an experimental batch mode to create as many videos as you want with different prompts, with only 1 run.

If you run into any issues, feel free to open an issue and I’ll do my best to help troubleshoot. Be as specific as possible. For example, if you get an error message at any point you need to include this in the issue, alongside operating system and computer specs.

## Examples
<img src="./examples/self_portrait_of_an_AI.png" width="512px"></img>

<img src="./examples/terrarium.jpg" width="256px"></img><img src="./examples/abandoned_shopping_mall.jpeg" width="256px"></img>

<img src="./examples/retro_playroom.jpg" width="256px"></img><img src="./examples/purity_and_grace.png" width="256px"></img>

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
Now you want to download, then run, the Linux Anaconda installer as follows. If you’re following this much later than March 2022 you can replace the url below with the latest version from [the Anaconda website](https://www.anaconda.com/products/individual?modal=nucleus).

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

Finding the above took a lot of trial and error. The difficulty was finding a pytorch and cudatoolkit combination which works with pytorch3d (required later). The above worked for me.

Now install some other dependencies:

    conda install jupyter pandas requests matplotlib
    conda install opencv -c conda-forge

### Option 1 - Similar to Colab, Easy
Option 1 for how to actually run the code and get images / video. Option 1 involves downloading a .ipynb, that is lightly modified from the colab notebook, editing cells and editing them inside the notebook environment.
#### Option 1, Step 5: Download the notebook
We’ll be working within a jupyter notebook version of the colab notebook. (I’m currently working on a cleaner interface, make sure to star and watch the repo to see when this goes live). 

Download the jupyter notebook in this repo. If you know how, clone the repo directly into your Ubuntu distribution. To make this guide as easy to follow as possible, I’ll also show an easier way. 

In your Ubuntu terminal, type:

    explorer.exe .

This will open your Ubuntu directory in Windows Explorer! Find a location you want to download the notebook to, maybe create a new folder for it.

On my github repository, click “code” then download zip. Extract the zip, and copy the .ipynb file to the desired folder **in Ubuntu**. If typed explorer.exe earlier, you’ll have an Ubuntu folder open in Explorer, so you can drag and drop into this folder. 

#### Option 1, Step 6: Run the notebook!

In your Ubuntu terminal, run:

    jupyter notebook

You might notice that this doesn’t automatically open jupyter in your browser. That’s okay! Just look for the URL starting with localhost, copy this, and paste it into your browser on Windows.

This should open jupyter in your browser! Now navigate to the folder where you have placed your jupyter notebook, and open it. Run the cells, one by one. They should install further required dependencies and download all the models for you. Along the way, you can change any settings you would like. One of the last cells asks for “text_prompts”, which you can specify to create whatever you wish!

### Option 2 - Batch mode command line, create multiple videos in 1 run, more advanced, still experimental
Option 2 for how to actually run the code and get images / video. This involves setting up a folder with settings files, which the notebook will work through 1 by 1. This will allow you to specify prompts for as many different videos as you would like, and create them all with a single run of a notebook. 

Some options must be specified once, and will be used for all items in the queue. Set these in "queue/master_settings.txt":
* diffusion_model
* use_secondary_model
* diffusion_steps
* ViTB32
* ViTB16
* ViTL14
* RN101
* RN50
* RN50x4
* RN50x16
* RN50x64
* width
* height
* init_image
* translation_x
* translation_y
* translation_z
* rotation_3d_x
* rotation_3d_y
* rotation_3d_z
* turbo_steps
* frames_skip_steps

Options that can be specified for each video are as follows. Must be specified in "queue/queue_1.txt", "queue/queue_2.txt" etc. Files can be created while the script is running, without interruption!
* text_prompts
* image_prompts
* max_frames
* steps
* seed

Note that this is currently experimental, and intended for creating a series of videos (not images). You are welcome to submit issues for bugs / feature requests, or even your own pull requests if you want to improve this ;)

Also, for my uses, fixing all those features works fine. If there are features you would like to be able to change between runs in the queue that you can't currently, feel free to start an issue or pull request.
#### Option 2, Step 5:
clone the repo into your Ubuntu installation. If you don't know how to do this, click "code" and "download zip" on this repo. Copy the entire repo into a folder in your Ubuntu environment. This is usually somewhere like "\\wsl$\Ubuntu\home\USERNAME\". You can access it easily by typing explorer.exe . in your Ubuntu window.

One of the folders you copied should be called "queue". Open this, and specify what settings you want in "master_settings". Then specify what prompts you want in each video, in separate files in this same folder. They should ve named "queue_1.txt" onwards, without any gaps. 

You can synthesize the queue files from the command line, simply navigate to the cloned repository and type:

     jupyter nbconvert --execute --to notebook --inplace Disco_Diffusion_v5_Turbo_w_3D_animation_from_queue.ipynb

The above will run all cells in the jupyter notebook from the command line. You can also run them in jupyter if you prefer. See option 1 for instructions on how to run the jupyter notebook if you'd like. 

That should be it! This should start creating images in your queue, 1 by 1. 

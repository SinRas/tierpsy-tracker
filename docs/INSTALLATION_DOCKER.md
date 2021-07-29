# Installing Tierpsy with Docker

You can now download and use Tierpsy Tracker as a Docker image.

If you are not familiar with Docker, this is as if you were given a brand new
computer with Tierpsy Tracker pre-installed.
After the installation, you will only have to click on a Desktop icon to run
Tierpsy.



## How to install

You will need three things to use Tierpsy with Docker:
1. Docker itself
2. The docker image for Tierpsy Tracker
3. A program to show Tierpsy's user interface.\
   We recommend [VcXsrv](https://sourceforge.net/projects/vcxsrv/) for Windows
   and [XQuartz](https://www.xquartz.org/) for macOS.

### Installation on Windows 10
**Note:**
*While you do not need to have admin rights to run Tierpsy on Docker in Windows,
you will need them for the installation process.*

This guide looks long, but it's just *that* thorough. An average time to run through this is 20 minutes.

#### 1. Install Docker

Download and run the installer from [here](https://desktop.docker.com/win/stable/amd64/Docker%20Desktop%20Installer.exe) *[Needs admin rights]*.

You can keep the default installation settings.
If prompted about it, make sure that Docker is installed in `C:\Program Files\Docker`, and if you're asked to install the "required Windows components for WSL 2" say yes.
Restart the computer when required to do so.

After restarting, you'll likely be told that the "WSL2 installation is incomplete".
Follow the link in the warning message and install the "WSL2 Linux Kernel".
[Here](https://medium.com/@tushar0618/installing-docker-desktop-on-window-10-501e594fc5eb#:~:text=5.%20Once%20your,approve%20this%20installation) is a short guide with screenshots.

Then restart the computer again.

>If you are not an administrator of the computer, your user needs to be added to the local group named "docker-users".\
>From a PowerShell or command prompt with Administrator rights, run `net localgroup docker-users your_username_here /add`

Now run the Docker Desktop app from the start menu.


> At this point you *may* be prompted with a nasty looking error message about needing to enable virtualisation in the BIOS. How to do this is highly dependent on the computer model, but the main steps are:
> + turn off your computer
> + turn it back on
> + press F2/DEL to enter the BIOS when prompted
>   + the right key to press might change, the computer will tell you which one to press.
>   + pay close attention as the message could be on screen for a very short time if you have an SSD
>   + spamming the right key during the startup is often a successful strategy
> + in the BIOS, find the options related to the CPU (could be named Northbridge, or Chipset)
>   + this might be under an **Advanced Options** section
> + find the option that allows you to enable hardware virtualisation
>   + be on the lookeout for options such as **Hyper-V**, **Vanderpool**, **SVM**, **AMD-V**, **Intel Virtualization Technology** or **VT-X**
>   + if you see options for **Intel VT-d** or **AMD IOMMU** enable those as well
> + save the changes and exit the BIOS
> + start the computer as usual

For reference, you can find the full documentation on how to install Docker on windows
[here](https://docs.docker.com/docker-for-windows/install/).

#### 2. Get the Tierpsy image

If you have a Docker Hub account, or are ok with creating one, you can look for the
`lferiani/tierpsy-tracker` image in the Docker Desktop app.

Otherwise, open Powershell:

![Start powershell](https://user-images.githubusercontent.com/33106690/127387980-d2199548-d09e-4293-b110-fdd9ee6c1af3.png)

Then type this command in the window that opens:
```
docker pull lferiani/tierpsy-tracker
```
![Pull image](https://user-images.githubusercontent.com/33106690/127388016-f3e80f4f-73f1-48d2-b244-27f72a542c0b.png)

press `Enter` on your keyboard and wait for the download to be finished. This will take a few minutes, but you can move to the next step in the meantime.

![Image downloading](https://user-images.githubusercontent.com/33106690/127388063-5849bf82-9eb0-4b9f-afaa-9d27ca93024b.png)


#### 3. Install VcXsrv

Download and run the [installer](https://sourceforge.net/projects/vcxsrv/files/latest/download) *[needs admin rights]*.

Follow the installation wizard, making sure VcXsrv is being installed in `C:\Program Files\VcXsrv`

![vcxsrv_1](https://user-images.githubusercontent.com/33106690/127391065-b8d68844-d2e5-4652-8b44-84d4fcb11f25.png)|![vcxsrv_1](https://user-images.githubusercontent.com/33106690/127391067-49dfe117-e41b-41b9-bb66-1039d06ea594.png)
:---:|:--:


#### 4. [Optional] Create a Desktop Launcher

Download [this](https://github.com/luigiferiani/tierpsy-tracker/files/6896343/tierpsy.zip) zip, exctract the PowerShell script it contains, and save it on your Desktop.

There is one modification you might want to do. When Tierpsy runs in Docker, it cannot access all of your hard drives. You need to configure what folders (or entire disks) Tierpsy has access to.

By default, the launcher allows Tierpsy to see the entirety of the `D:\` drive.
This is also the folder that Tierpsy starts in. If you want to modify this behaviour, just open the launcher with a text editor, and modify line 10 by putting the path of the folder you want Tierpsy to access.
You'll have to substitute any `\` symbol in your path of choice with `/` symbols. The path should be enclosed in double quotation marks, e.g. `"c:/your/path/here"`.

![Screenshot (11)](https://user-images.githubusercontent.com/33106690/127395925-597545a9-592c-4002-9e82-8d5618c31d8e.png)

Once you're done, save your changes, close the editor, and you can then `right click --> Run with PowerShell` to launch Tierpsy Tracker (no, double-click does not work - this is by design from Microsoft).

![run w/ PS](https://user-images.githubusercontent.com/33106690/127393995-789982dc-e087-4549-b047-c1c88820b52e.png)

> You _might_ have to allow PowerShell to run scripts first. To do this, open a PowerShell as Administrator\
> (Windows key, type `PowerShell`, then right click on the best match and `Run as Administrator`)\
> and type `Set-ExecutionPolicy RemoteSigned`, then press `Enter` on your keyboard.

##### Why do I need a Desktop Launcher?
Strictly speaking, you do not need a Launcher, but it automates a few boring actions, and sets some parameters that are quite important for Tierpsy Tracker to run smoothly. See the next section if you prefer to start the image manually.

##### What does the launcher do exactly?
3 things:
- checks if VcXsrv is running, if not it starts it
- checks if Docker is running, if not it starts it
- waits for Docker to be up, and starts a Docker container from the Tierpsy image, setting a bunch of parameters needed to run properly.

#### 4. [Alternative] Start Tierpsy Tracker manually

If you prefer not to use the Launcher, you just need to run a few steps manually.

First, start VcXsrv from the Start menu. Use the following settings:

![Screenshot (3)](https://user-images.githubusercontent.com/33106690/127397026-a86ccf66-0569-4b52-8e9c-28e676e96259.png)|![Screenshot (4)](https://user-images.githubusercontent.com/33106690/127397032-db4db93f-a7fa-46bb-aa1b-a62ef9e04408.png)|![Screenshot (5)](https://user-images.githubusercontent.com/33106690/127397033-ffd16ec3-e2a8-4f5c-8181-0860982ae395.png)
:---:|:---:|:---:

Once it's running, you'll have a little icon with a black X in the application tray.

Then start the Docker Desktop app from your Start Menu or Desktop shortcut, wait until the app says it's running properly.

Open PowerShell, and run this:
```
docker run -it --rm -e DISPLAY=host.docker.internal:0 -v d:/:/DATA --sysctl net.ipv4.tcp_keepalive_intvl=30 --sysctl net.ipv4.tcp_keepalive_probes=5 --sysctl net.ipv4.tcp_keepalive_time=100 lferiani/tierpsy-tracker
```


### Installation on macOS

> This was tested on a MacBook Pro running Mojave. Might not work on machines with an M1 chip.

#### Minimal installation instructions

- Install [Xquartz](https://www.xquartz.org/)
- Install [Docker](https://docs.docker.com/docker-for-mac/install/)
- make sure Docker is running (icon in status bar)
- In Docker Desktop open the Settings (the cog icon on the top right), then open the Resources tab, and move the CPU, Memory, and swap sliders to the right. _For reference, I'm using as many processors as I can, 12 GB out of 16GB of Memory, and 2GB of Swap._
- open a Terminal, and download the Tierpsy image:
  ``` bash
  docker pull lferiani/tierpsy-tracker
  ```

#### Create a launcher script

Open your text editor of choice, copy paste the following code:
``` bash
#!/usr/bin/env bash

xlogo &
killall xlogo

xhost +localhost

docker run \
    -it \
    --rm \
    -e DISPLAY=host.docker.internal:0 \
    -v /Volumes/data:/DATA \
    --sysctl net.ipv4.tcp_keepalive_intvl=30 \
    --sysctl net.ipv4.tcp_keepalive_probes=5 \
    --sysctl net.ipv4.tcp_keepalive_time=100 \
    --hostname tierpsydocker \
    tierpsy-tracker

```

> Make sure you change `/Volumes/data` for the folder you want Tierpsy to access.

Then save the script on your Desktop, as `run_tierpsy_docker` (no extension).

Open a Terminal, run \
`chmod +x ~/Desktop/run_tierpsy_docker`
to make the script executable.


#### To start Tierpsy

- Launch Docker
- double click on the `run_tierpsy_docker` launcher that is on the Desktop


## Using Tierpsy in Docker

After launching Tierpsy, wither with a Desktop launcher or from the command line,
you'll be greeted by a simple landing page, that lists the three main commands.

![text splash](https://user-images.githubusercontent.com/33106690/127402583-eb7ca247-09d8-4803-a522-db495014a51b.png)


- ### `tierpsy_gui`
This should be your go-to command. Opens the small application windows from where you can choose what step of the tierpsy analysis you want to undertake. See [How to Use](docs/HOWTO.md) for detailed instructions.

![qt splash](https://user-images.githubusercontent.com/33106690/127402654-78e2ccc5-1032-47b0-9cce-8377b1901de4.png)

- ### `is_tierpsy_running`
Running Tierpsy Tracker with Docker, there is a chance that if the GUI is not interacted with for a long time,
it might time out. We haven't seen this happen during the analysis, and it does not affect the processing of videos, which just keeps happening in the background.

If you return to your computer after a few hours, and in place of the GUI find a message reading\
_The X11 connection broke (error 1). Did the X11 server die?_\
don't panic, this should not be affecting any analysis. So you can press Enter a couple of times, and just run the command `is_tierpsy_running`.

This will scan the active processes, and tell you how many, if any, videos are being processed in the background. \
If the reading is 0, then it is likely that Tierpsy had finished analysing the data, and the command will explan how to double-check that all of your files have been processed.

- ### `tierpsy_process`
This utility allows you to bypass the GUI entirely, if you cannot or prefer not to use one.
Please read the documentation that appears with `tierpsy_process --help` and do not hesitate to contact us for help.



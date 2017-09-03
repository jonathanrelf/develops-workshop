# Devel-ops Packer Workshop

## Information
This workshop will make use of Packer to produce a VirtualBox image from an Ubuntu Server ISO installation ISO. It also highlights how you can start to use *provisioners* to pre-install software and ensure the virtual machine image is always up to date.

To save waiting for the main Ubuntu Server ISO to download at the talk, I'm suggesting downloading this beforehand to save time. Some internet connectivity will still be necessary to complete further operations so make sure you have access to the internet as well.

## Installing the required software
1. Clone this Git repository to your computer.
1. Download and install [Packer](https://www.packer.io) 
    * Windows & Linux users should download a suitable binary from [https://www.packer.io/downloads.html](https://www.packer.io/downloads.html "Packer downloads").
    * Mac Users should install Packer using [Homebrew](https://brew.sh) using  

            brew install packer
    
    Follow the *[installation instructions](https://www.packer.io/docs/install/index.html "Packer installation instructions")* to ensure the downloaded binary is able to be run from your command-line / terminal. 
1. Download and install [Vagrant](https://www.vagrantup.com) from [https://www.vagrantup.com/downloads.html](https://www.vagrantup.com/downloads.html "Vagrant downloads") suitable for your computer. 
    
    Run the downloaded binary to install Vagrant and follow the instructions. Supporting *[installation instructions](https://www.vagrantup.com/docs/installation/ "Vagrant installation instructions")* are available.
1. Download and install [VirtualBox](https://www.virtualbox.org) from [https://www.virtualbox.org/wiki/Downloads](https://www.virtualbox.org/wiki/Downloads "VirtualBox downloads") picking the binary suitable for your computer. 

    Run the downloaded binary to install VirtualBox and follow the instructions. Supporting *[installation instructions](https://www.virtualbox.org/manual/ch01.html#intro-installing "VirtualBox installation instructions")* are available.
1. Download the ISO for [Ubuntu Server 16.04.3 LTS](https://www.ubuntu.com/server) by visiting this [download link](https://www.ubuntu.com/download/server/thank-you?country=GB&version=16.04.3&architecture=amd64 "Ubuntu Server 16.04.3 ISO download").
1. Copy (or move) the Ubuntu Server ISO file in to the packer directory of your local copy of this Git repository. 

## Building the basic VirtualBox image
1. Open a terminal (Linux & macOS) or Command Prompt (Windows)
1. Change directory to the *packer* directory of your local copy of this Git repository. E.g.

        cd ~/Downloads/develops-workshop/packer

    replacing the path with where you downloaded this Git repository to.
     
1. Tell **Packer** to start building a new VirtualBox image by executing  
    
        packer build virtualbox.json

   * This will now take about 10-15 minutes to complete. The *preseed.cfg* file is shared with the virtual machine that's building the image via a temporary web share that Packer hosts. This allows the unattended configuration of the Ubuntu setup process without needing to interact with a GUI.  

1. At the end of the process an *ubuntu.box* file will be saved in the same directory.

Congratulations! You've just created your first pipeline for creating machine images using Infrastructure As Code principles. 

## Adding server features
We'll now make use of the *Provisioner* support in Packer to install optional packages to the image before it gets finalised and output as a base image. 

We'll make use of the simplest *provisioner*, the Shell provisioner. Here we'll add a LAMP server. 

1. Add the following block to the virtualbox.json file 

        "provisioners": [{
            "type": "shell",
            "inline": [
                "sleep 30",
                "sudo apt-get update",
                "sudo debconf-set-selections <<< 'mysql-server mysql-server/root_password password your_password'",
                "sudo debconf-set-selections <<< 'mysql-server mysql-server/root_password_again password your_password'",
                "sudo apt-get install -y lamp-server^"
            ]
        }]
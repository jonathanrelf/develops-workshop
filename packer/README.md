# Devel-ops Packer Workshop

## Requirements
1. [Packer](https://www.packer.io) - [downloads](https://www.packer.io/downloads.html "Packer downloads") with *[installation instructions](https://www.packer.io/docs/install/index.html "Packer installation instructions")*
1. [Vagrant](https://www.vagrantup.com) - [downloads](https://www.vagrantup.com/downloads.html "Vagrant downloads") with *[installation instructions](https://www.vagrantup.com/docs/installation/ "Vagrant installation instructions")*
1. [VirtualBox](https://www.virtualbox.org) - [downloads](https://www.virtualbox.org/wiki/Downloads "VirtualBox downloads") with *[installation instructions](https://www.virtualbox.org/manual/ch01.html#intro-installing "VirtualBox installation instructions")*
1. [Ubuntu Server 16.04.3 LTS ISO](https://www.ubuntu.com/server) - [download](https://www.ubuntu.com/download/server/thank-you?country=GB&version=16.04.3&architecture=amd64 "Ubuntu Server 16.04.3 ISO download")

## Information
This workshop will make use of Packer to produce a VirtualBox image from an Ubuntu Server ISO installation ISO. It also highlights how you can start to use *provisioners* to pre-install software and ensure the virtual machine image is always up to date.

To save waiting for the main Ubuntu Server ISO to download at the talk, I'm suggesting downloading this beforehand to save time. Some internet connectivity will still be necessary to complete further operations so make sure you have access to the internet as well.

## Building the basic VirtualBox image
1. Install the above requirements to your computer, ensuring you can run Packer and Vagrant from your command-line / terminal envrionment.
1. Download and move/copy the Ubuntu Server 16.04.3 ISO into the root of this directory after downloading it. 
1. Clone this Git repository to your local machine.
1. Open your command-line / terminal environment and change your current working directory to the packer directory from the root of this downloaded Git repository.

1. Start packer building the VirtualBox image by executing  
    
        packer build virtualbox.json

   * this will now take about 10-15 minutes to complete. The *preseed.cfg* file is shared with the image via a temporary web share that Packer hosts. This allows the unattended configuration of the Ubuntu setup process without needing to interact with a GUI.  

1. At the end of the process an ubuntu.box file will be saved.

## Adding server features
**If you wish to iterate with provisioners faster, I'd suggest taking the base image from the first section and move to the examples with Vagrant.**

We'll now make use of the *Provisioner* support in Packer to install optional packages to the image before it gets finalised and output as a base image. 

We'll make use of the simplest *provisioner*, the Shell provisioner. I'm suggesting adding a LAMP server, but feel free to add or replace anything else you may fancy. 

1. Add the following block to the virtualbox.json file 

        "provisioners": [{
            "type": "shell",
            "inline": [
                "sleep 30",
                "sudo apt-get update",
                "sudo apt-get install -y lamp-server"
            ]
        }]

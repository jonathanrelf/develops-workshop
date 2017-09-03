 # Devel-ops Vagrant Workshop

## Requirements
1. [Vagrant](https://www.vagrantup.com) - [downloads](https://www.vagrantup.com/downloads.html "Vagrant downloads") with *[installation instructions](https://www.vagrantup.com/docs/installation/ "Vagrant installation instructions")*
1. [VirtualBox](https://www.virtualbox.org) - [downloads](https://www.virtualbox.org/wiki/Downloads "VirtualBox downloads") with *[installation instructions](https://www.virtualbox.org/manual/ch01.html#intro-installing "VirtualBox installation instructions")*
1. *optional* Ubuntu Box image from Packer workshop

## Information
In this workshop we will make use of Vagrant to show how  Infrastructure As Code can be developed and tested locally on your development environment. 
We will start by spinning up a test server and then move on to provisioning some additional server features. 

## Running an instance of your chosen machine image
In this section you will use Vagrant to spin up a test server on your local development environment.
1. Open your command-line / terminal environment and change your current working directory to the vagrant folder from the root of this downloaded Git repository.
1. Execute the command  
        
        vagrant init
1. If successful, the following message will be displayed:  

        A `Vagrantfile` has been placed in this directory. You are now ready to `vagrant up` your first virtual environment! Please read the comments in the Vagrantfile as well as documentation on `vagrantup.com` for more information on using Vagrant.
1. Open the Vagrantfile in your favourite text editor
1. Amend the config.vagrant.box variable.
    - if using the Packer image we built in the previous workshop example use:  

            config.vm.box = "../packer/ubuntu.box"
    - if you'd rather use a pre-built image from the internet, you can specify Ubuntu 16.04 LTS (found on  https://app.vagrantup.com/boxes/search) by using: 

            config.vm.box = "ubuntu/xenial64"
    This will import either box definition in to Vagrant for use in the future.
1. It's now time to spin up your box by typing:  

        vagrant up
    This will spin up the virtual machine in 'headless mode' by default so you won't see VirtualBox running in the foreground.

1. You can now connect to your 'headless server' by typing  

        vagrant ssh

    You can exit this session at any time by executing:  

            exit
1. To stop and destroy the virtual server you just created,  execute:  

        vagrant destroy
    and confirm your choice by selecting Y for yes. 
    This is useful when you want to prove everything works from scratch.

## Adding a provisioner
1. Uncomment the section at the bottom of the Vagrantfile from  

        config.vm.provision "shell", inline: <<-SHELL
    to  

        SHELL
1. Amend the shell script to install a LAMP stack to look as:  

            apt-get update
            sudo debconf-set-selections <<< 'mysql-server mysql-server/root_password password your_password'
            sudo debconf-set-selections <<< 'mysql-server mysql-server/root_password_again password your_password'
            apt-get install -y lamp-server^

     amending the *your_password* to a value of your choice (this is not recommended practice but is a quick way of getting started).
1. Bring back up the virtual machine with  

        vagrant up
    
    This will additionall run the Shell provisioner this time and should download all the packages necessary to install a LAMP server on your machine.
1. The server will now be ready and listening for incoming web requests on TCP Port 80. We can verify this by connecting to the running Virtual server by executing

        vagrant ssh
    and then running the command

        netstat -an | grep 80
    which will show it LISTENing however from outside of the virtual machine we cannot browse to it.
1. To map the *guest* port to the *host* machine, uncomment the line:  

        config.vm.network "forwarded_port", guest: 80, host: 8080
    and save the file. 
1. As this is a change to how the virtual machine is hosted, you will need to *destroy* and *up* again using vagrant to map the ports. 
1. You should now be able to browse to http://localhost:8080 on your local development machine and see the default Ubuntu webpage. 

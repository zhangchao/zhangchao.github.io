
# How to use Vagrant Docker and boot2docker! #
#
#

- **vagrant** 
-
**Install:**

	 $ vagrant box add ubuntu-14.04-amd64-for-docker https://oss-binaries.phusionpassenger.com/vagrant/boxes/latest/ubuntu-14.04-amd64-vbox.box
	 or 
	 $vagrant box add my-box file:///d:/path/to/file.box 
	 $ vagrant init ubuntu-14.04-amd64-for-docker
	 $ vagrant up

**How to start GUI:**

1. vagrant ssh
2. Modify Vagrantfile:

	    config.vm.provider :virtualbox do |vb|	    
	      vb.gui = true	    
	    end

3. sudo apt-get install xfce4
4. sudo startxfce4&

		$ sudo vim /etc/X11/Xwrapper.config and edit it to allowed_users=anybody.
		$ sudo apt-get install -y xfce4 virtualbox-guest-dkms virtualbox-guest-utils virtualbox-guest-x11
		$ sudo VBoxClient-all
		vagrant user, with $ startxfce4&.


- **docker** 
-
> sudo vi /etc/defaults/docker.io (*docker*)
> 
> export HTTP_PROXY=http://your.proxy.name:8080
> 
> export HTTPS_PROXY=http://your.proxy.name:8080
> 
> sudo service docker.io (*docker*) restart

*ERROR: Get https://index.docker.io/v1/repositories/ubuntu/images: dial tcp 162.242.195.84:443: connection timed out*

    **How to Install Docker in the ubuntu 12**
 	1. sudo apt-get update
 	2. sudo apt-get install linux-image-generic-lts-raring linux-headers-generic-lts-raring
 	3. curl -sSL https://get.docker.io/ubuntu/ | sudo sh


	**How to Build Image with Dockfile**
	1. Create Docker File
	2. sudo docker build -t="zhangchao/quickdev" .

	


> ** Basic Command **

    docker run -d -p 27017:27017 --name mongodb dockerfile/mongodb
    or
    docker run -d -p 27017:27017 -p 28017:28017 --name mongodb dockerfile/mongodb mongod --rest --httpinterface
    
    docker run -it --rm --link mongodb:mongodb dockerfile/mongodb bash -c 'mongo --host mongodb'
    
    VBoxManage modifyvm "boot2docker-vm" --natpf1 "guestmongodb,tcp,127.0.0.1,27017,,27017"
    
    docker run -it --rm dockerfile/python python
    
    docker run -it --rm dockerfile/nodejs-bower-grunt node
    
    docker run -it --rm dockerfile/nodejs-bower-grunt npm
    
    docker run -it --rm dockerfile/nodejs-bower-grunt bower
    
    docker run -it --rm dockerfile/nodejs-bower-grunt grunt
    
    docker build -t="zhangchao/quickdev" .
    




- **boot2docker**
-
> sudo vi /var/lib/boot2docker/profile 
> 
> export HTTP_PROXY=http://your.proxy.name:8080
> 
> export HTTPS_PROXY=http://your.proxy.name:8080



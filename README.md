# CoreOSLoadBalancing
Load Balancing with CoreOS

# Vagrant Files
This has the VagrantFile and two extra files (config.rb and user-data) to supplement it. 

VagrantFile has the configuration details, 

Config.rb can be used as an alternate to directly editing the VagrantFile. At Vagrant up, the VagrantFile will run config.rb and override it's default values with the ones specified in it.

This has cloud configuration settings, like etcd discovery tokens, and explains the provisioning details to VagrantFile.

# Nginx Plus Docker
Contains the dockerfile which tells docker how to make the container, and backend.conf which contains nginx configuration, and the authentication keys (which you must place yourself, these are just samples).

# CoreOS Units
Has the unit files used by fleetctl to start services.

# Wordpress Docker-Compose
Has the yaml file which is used by Compose to create all docker containers needed for running a wordpress site.

# How it works
Steps-
1) Use either Vagrant or AWS to run 3 to 4 CoreOS instances. In the former case, utilize VagrantFiles provided.
2) Regardless of the above choice, use the nginx docker file to create the docker container. This needs to be done outside coreOS. I used Ubuntu 14.04 for this.
3) Load the docker container created in the last into the coreOS instances.
4) Run the CoreOS units using fleetctl.

We now have a fully functional nginx load balancer on one of the CoreOS instances and multiple simple nginx pages in the backend.

# Bonus:
Use docker-compose on the given yml file to quickly run wordpress on a coreOS instance. Docker-compose does all the heavy-lifting for you and you have multiple containers created at once (mysql, frontent, etc.). The unit files for this aren't provided here.

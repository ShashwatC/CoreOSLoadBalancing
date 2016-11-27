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

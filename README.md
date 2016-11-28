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

# What do we get?

1) High availabilty cluster - Whenever any node goes down, fleetctl will work with etcd and migrate the affected service to any other node which doesn't conflict with the service. 

2) Nginx Plus has support for on the fly reconfiguarion, and detects machines which goes down and other machines which take their place.

3) Load balancing - Nginx Plus has out of the box support for round-robin, ip_hash and least connected algorithms for load-balancing. 

# What we tested

1) We took down coreos machines and fleet moved the services to another coreos instance.
2) When this occurs, nginx plus reconfigures itself with the new web servers and directs the requests accordingly.
3) Nginx Plus load balances the requests and distributes it across all the web servers which are up and running.

# Change load balancing technique

This requires a simple change in the backend.conf file

Current file has the following relevant section-

upstream backend {
    zone backend 64k;
}

This defaults to round-robin. To get least connected algorithm (the server with least traffic gets new connections) or ip_hash (so that same client always connects to the same server), you just need to add one line.

upstream backend {
    least_conn;
    zone backend 64k;
}

upstream backend {
    ip_hash;
    zone backend 64k;
}

# Nginx On The Fly
You can use the settings icon on the Nginx Dashboard and make changes on the fly.

<img src="https://s18.postimg.org/5dt9xshgp/Screenshot_from_2016_11_28_12_57_30.png">

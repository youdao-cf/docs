#Gorouter Depolyment Guide on CentOS6

Gorouter is the next-generation component of CloudFoundry.
This guide describes how to set up gorouter module individually on CentOS6. 

## Preperation
### Go runtime environment
Gorouter is implemented by go languague, so you should install go runtime libs first. Please refer to offical page [Go Install](http://golang.org/doc/install#install).

### Git & Ruby 

### Nats server
Make sure you have setup nats-server.

## Setup
### Get source code

    git clone https://github.com/cloudfoundry/gorouter.git
	cd gorouter
	git submodule update --init
	
### Build router
	
	./bin/go install router/router

### Modify config
You may use your customed yml file, but here we modify config/example.yml to meet requirements. 
	
	vi config/example.yml

* Change nats server url to your own's.
* Gorouter defines default port as 8081, but we usually want it to listen on 80 for all input requests.  

### Start up 

	./bin/router -c config/example.yml
     

##Notes
* Gorouter code entry : gorouter/router/router/main.go
* Message Pub&Sub
	* Publish: router.start
	* Publish: vcap.component.announce
	* Subscribe: router.register
	* Subscribe: router.unregister
	* Subscribe: vcap.component.discover

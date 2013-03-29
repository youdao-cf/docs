#UAA Depolyment Guide on CentOS6

The UAA is the identity management service for Cloud Foundry.
This guide describes how to set up UAA individually on CentOS6. 

## Preperation
### Java & Maven 
UAA is mainly implemented in Java and organized via Maven3, so please ensure [Java](http://www.oracle.com/technetwork/java/javase/downloads) (version >= 1.6) and [Maven](http://maven.apache.org/download.cgi) (version > 3.0.4) are installed. And add their "bin" to Path env.

### Git & Ruby 

### Nats server
Make sure you have setup nats-server.

### Postgresql
Install postgresql before UAA locally or in some server. Change all the "ident" to "md5" on column METHOD of the conf file /var/lib/pgsql/data/pg_hba.conf and add a new line "host uaa root 0.0.0.0/0 md5".
Restart postgresql service via "service postgresql restart".

## Setup
### Get source code

    git clone https://github.com/cloudfoundry/uaa.git
	cd uaa
	
### Install gems

	bundle install

### Modify uaa default pom.xml
	
	vi /home/zhaodch/git/uaa/uaa/pom.xml
Comment line 40 as 
	<!--<CLOUD_FOUNDRY_CONFIG_PATH>${project.basedir}/src/test/resources/test/profiles/vcap</CLOUD_FOUNDRY_CONFIG_PATH>-->
> This line is under mvn profile vcap which is used by bin/uaa by default. We comment it to support our customed CLOUD_FOUNDRY_CONFIG_PATH in execution later.

### Build UAA
	
	mvn install

### Modify config
We recommand to make use of config/uaa.yml. But it is initialized nearly empty, we should define our configuration as below.
	
	pid: /var/vcap/sys/run/uaa.pid
	mbus: nats://[user]:[password]@[nats server ip]:4222
	uaa:
		host: [current server ip]
		port: 8080   (you'd better not change it as there are some hard code 8080 in uaa source)

	tags:
		component: uaa

	jvm_args: -Xmx512m
	spring_profiles: postgresql

	batch:
		user: batch_user
		password: batch_password

	database:
		url: jdbc:postgresql://[postgresql url]:5432/uaa

	cloud_controller:
		database:
			url: jdbc:mysql://[mysql url]:3306/[database name]   (other dbs are ok)
			username: [yours]
			password: [yours]

	jwt:
		token:
			signing-key: cloudcontrollersecret
			verification-key: cloudcontrollersecret

	oauth:
		clients:
			admin:
				authorized-grant-types: client_credentials
				scope: clients.read,clients.write,clients.secret,uaa.admin,scim.read,scim.write
				authorities: clients.read,clients.write,clients.secret,uaa.admin,scim.read,scim.write
				id: admin
				secret: adminsecret
				resource-ids: none
		cloud_controller:
				authorized-grant-types: client_credentials
				authorities: scim.read,scim.write,password.write,uaa.resource
				id: cloud_controller
				secret: cloudcontrollersecret
				resource-ids: none
				access-token-validity: 604800
		vmc:
				authorized-grant-types: implicit
				authorities: uaa.none
				id: vmc
				resource-ids: none
				redirect-uri: http://uaa.cloudfoundry.com/redirect/vmc,https://uaa.cloudfoundry.com/redirect/vmc
				access-token-validity: 604800

>Please delete all the words within "()", they are just some hints not config.

### Start up 

	CLOUD_FOUNDRY_CONFIG_PATH=/[your git absolute path]/uaa/config/ ./bin/uaa

##Notes
* [UAA managing-user guide](http://cloudfoundry.github.com/docs/running/managing-users/)
* User rest api:
	* DELETE /users/{email}
	* GET /users
	* GET /users/{email}
	* POST /users
	* OST /users/{email}/tokens
	* PUT /users/{email}

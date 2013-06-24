#UAA Depolyment Guide on CentOS6

The UAA is the identity management service for Cloud Foundry.
This guide describes how to set up UAA individually on CentOS6. 

## Preperation
### Java & Maven 
UAA is mainly implemented in Java and organized via Maven3, so please ensure [Java](http://www.oracle.com/technetwork/java/javase/downloads) (version >= 1.6) and [Maven](http://maven.apache.org/download.cgi) (version > 3.0.4) are installed. And add their "bin" to Path env.

### Git & Ruby 

### Nats server
Make sure you have setup nats-server.

### UAA Database
You can choose postgresql or mysql. Please install one before UAA locally or in some server.
If you use Postgresql, please change all the "ident" to "md5" on column METHOD of the conf file /var/lib/pgsql/data/pg_hba.conf and add a new line "host uaa root 0.0.0.0/0 md5". Restart postgresql service via "service postgresql restart".

### CloudController Database
Please make sure your CloudController Database has support of *TRANSACTION*. If use mysql, you can use InnoDB as your engine.

## Setup
### Location
- Git Repo : https://github.com/youdao-cf/uaa.git
- Branch : dev_yae
- Commit No: edebb23e43d8c998e8dbcc09b3d173b2a696a7c3

### Get source code

    git clone https://github.com/youdao-cf/uaa.git
	cd uaa
	git checkout dev_yae
	git checkout edebb23e43d8c998e8dbcc09b3d173b2a696a7c3
	
### Install gems

	bundle install

### Modify uaa default pom.xml
	
	vi uaa/uaa/pom.xml
	
Comment line 40 as 
	<!--<CLOUD_FOUNDRY_CONFIG_PATH>${project.basedir}/src/test/resources/test/profiles/vcap</CLOUD_FOUNDRY_CONFIG_PATH>-->
> This line is under mvn profile vcap which is used by bin/uaa by default. We comment it to support our customed CLOUD_FOUNDRY_CONFIG_PATH in execution later.

### Build UAA
	
	mvn clean package -DskipTests=true

### Create uaa.pid

	create a file named uaa.pid, and fill it in with a identified process number such as "10460"
	
### Modify config
We recommand to make use of config/uaa.yml. But it is initialized nearly empty, we should define our configuration as below.
	
	pid: [absolute path of your uaa.pid file]
	mbus: nats://[user]:[password]@[nats server ip]:4222
	uaa:
		host: [current server ip]
		port: 8080   (you'd better not change it as there are some hard code 8080 in uaa source)

	tags:
		component: uaa

	jvm_args: -Xmx512m
	spring_profiles: postgresql [or mysql] (based on your uaa database) 
		
	database:
		url: jdbc:mysql://[mysql url]:3306/[database name] (or use postgresql url)
		username: [yorus]
		password: [yours]

	cloud_controller:
		database:
			url: jdbc:mysql://[mysql url]:3306/[database name]   (other dbs are ok)
			username: [yours]
			password: [yours]

	jwt:
		token:
			signing-key: [must match with cc side config]
			verification-key: [must match with cc side config]

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
			cf:
				authorized-grant-types: implicit
				authorities: uaa.none
				scope: uaa.admin,scim.read,scim.write,openid,password.write,cloud_controller.admin,cloud_controller.read,cloud_controller.write
				id: cf
				resource-ids: none
				redirect-uri: http://uaa.cloudfoundry.com/redirect/cf,https://uaa.cloudfoundry.com/redirect/cf
				access-token-validity: 604800
				autoapprove: true
			login:
				id: login
				secret: loginsecret
				authorized-grant-types: client_credentials
				authorities: cloud_controller.admin,loud_controller.write,uaa.admin,scim.read,scim.write,openid,password.write
				scope: cloud_controller.admin,cloud_controller.write,uaa.admin,scim.read,scim.write,openid,password.write

>Please delete all the words within "()", they are just some hints not config.


## Integrate with LDAP

We forked and modified a CloudFoundry module called LoginServer to integrate LDAP authentication. 
### Location
- Git Repo : https://github.com/youdao-cf/login-server
- Branch : dev_yae
- Commit No: eea770bdf4ff599547b316fdd04a6f7e118541f6

### Get source code

	git clone https://github.com/youdao-cf/login-server.git
	cd login-server
	git checkout dev_yae
	git checkout eea770bdf4ff599547b316fdd04a6f7e118541f6

### Config ldap

	vi src/main/webapp/WEB-INF/spring-servlet.xml (config ldap url and basedn in line 287 and 289)

### Build LoginServer

	mvn clean package -DskipTests=true

## Start up uaa and loginserver

1. install a tomcat in your env
2. copy the war of uaa (in uaa/uaa/target) to tomcat/webapps/ with name "uaa.war"
3. copy the war of loginserver (in login-server/target) to tomcat/webapps/ with name "login.war"
4. copy login-server/src/main/resources/login.yml to uaa/config/login.yml
5. Run command "CLOUD_FOUNDRY_CONFIG_PATH=/[your git absolute path]/uaa/config/ UAA_TOMCAT=/[your tomcat path]/tomcat/ nohup ./uaa/bin/uaa > uaa.log &"

##Notes
* [UAA managing-user guide](http://cloudfoundry.github.com/docs/running/managing-users/)
* User rest api:
	* DELETE /users/{email}
	* GET /users
	* GET /users/{email}
	* POST /users
	* OST /users/{email}/tokens
	* PUT /users/{email}

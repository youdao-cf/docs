## prerequisites

cloud_controller needs a database, so you'd better setup a mysql server at first. sqlite also work while developing.

    $ sudo yum  install mysql-devel
    $ sudo yum install postgresql-devel
    $ sudo yum install sqlite-devel
    

    

## config note

- local_route: fill with local ip e.g. 10.168.3.170
- nats_uri: the nats-server host and port
- db: fill with the pre if you are going to use mysql, the protocol string starts with mysql2, like 'mysql2://username:password@host:3306/dbname'
- uaa: make sure symetric_secret is the uaa jwt secret 

## location
Git Repo : https://github.com/cloudfoundry/cloud_controller_ng 
Branch : master
Commit No: 9d8c2a3bce46869cabcf25a70b7bdb22ed802849


## setup & start

    git clone https://github.com/cloudfoundry/cloud_controller_ng.git
    cd cloud_controller_ng 
	git checkout 9d8c2a3bce46869cabcf25a70b7bdb22ed802849
    git submodule update --recursive --init
    bundle install
    bundle exec rake db:migrate
    
    # if this is the first time  you run the cloud_controller, 
    # please add -m parameter to insert some necessery start data
    bundle exec bin/cloud_controller -c cloud_controller [-m]


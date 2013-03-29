
## prerequisites

cloud_controller needs a database, so you'd better setup a mysql server at first. sqlite also work while developing.


## config note

local_route: fill with local ip e.g. 10.168.3.170

nats_uri: the nats-server host and port

db: fill with the pre if you are going to use mysql, the protocol string starts with mysql2, like 'mysql2://username:password@host:3306/dbname'

uaa: make sure symetric_secret is the uaa jwt secret 


## setup & start

    git clone
    git submodule update --recursive --init
    cd cloud_controller_ng
    bundle exec rake db:migrate
    
    # if this is the first time  you run the cloud_controller, 
    # please add -m parameter to insert some necessery start data
    bundle exec bin/cloud_controller -c cloud_controller 


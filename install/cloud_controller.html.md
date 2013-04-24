## prerequisites

cloud_controller needs a database, so you'd better setup a mysql server at first. sqlite also work while developing.

    $ curl -O http://www6.atomicorp.com/channels/atomic/centos/6/x86_64/RPMS/atomic-release-1.0-14.el6.art.noarch.rpm
    $ sudo  rpm -Uvh atomic-release-1.0-14.el6.art.noarch.rpm
    $ rvm reinstall/install 1.9.3 --autolibs=3



## config note

- local_route: fill with local ip e.g. 10.168.3.170
- nats_uri: the nats-server host and port
- db: fill with the pre if you are going to use mysql, the protocol string starts with mysql2, like 'mysql2://username:password@host:3306/dbname'
- uaa: make sure symetric_secret is the uaa jwt secret 


## setup & start

    git clone
    git submodule update --recursive --init
    cd cloud_controller_ng
    bundle install
    bundle exec rake db:migrate
    
    # if this is the first time  you run the cloud_controller, 
    # please add -m parameter to insert some necessery start data
    bundle exec bin/cloud_controller -c cloud_controller [-m]


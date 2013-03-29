---
title: install cloud_controller
---


## config note

local_route: local ip

nats_uri: the nats-server host and port

db: if you are going to use mysql, the protocol string starts with mysql2, like 'mysql2://username:password@host:3306/dbname'

uaa: make sure symetric_secret is the uaa jwt secret


## setup & start

    git clone
    git submodule update --recursive --init
    cd cloud_controller_ng
    bundle exec rake db:migrate
    bundle exec bin/cloud_controller -c cloud_controller # if this is the first time  you run the cloud_controller, please add -m parameter to insert some necessery start data


---

## dependency

on centos 6:

    sudo yum install curl libcurl libcurl-devel

## config note
    # for upload url in staging ， is this value is missing , error will be reported.
    domain: vcap.me

## setup & start

    git clone
    cd dea_ng
    bundle install
    
    # do NOT add '-c' like other component
    bundle exec bin/dea config/dea.yml 

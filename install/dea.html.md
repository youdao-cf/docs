## dependency

on centos 6:

    sudo yum install curl libcurl libcurl-devel

## config note
    # for upload url in staging ， is this value is missing , error will be reported.
    domain: vcap.me

## setup & start

    git clone    
    cd dea_ng    （branch dev, commit f8f316eb907b439af8a9465132063b618f34b63e ）
    git submodule update --init --recursive
    bundle install
    
    # do NOT add '-c' like other component
    bundle exec bin/dea config/dea.yml 


## start directory server

    install go
    
    ./bin/go install runner
    
    ./bin/runner -conf ../config/dea.yml

## dependency

on centos 6:

    sudo yum install curl libcurl libcurl-devel

## config note
    # for upload url in staging ， is this value is missing , error will be reported.
    domain: vcap.me

## location
Git Repo : https://github.com/youdao-cf/dea_ng 
Branch : dev
Commit No: ffb8d9e95b8c005d84731a4aec955ad8353b1c7e
	
## setup & start

    git clone  https://github.com/youdao-cf/dea_ng.git
    cd dea_ng
	git checkout dev
	git checkout ffb8d9e95b8c005d84731a4aec955ad8353b1c7e
    git submodule update --init --recursive
    bundle install
    
    # do NOT add '-c' like other component
    bundle exec bin/dea config/dea.yml 


## start directory server

    install go
    
    ./bin/go install runner
    
    ./bin/runner -conf ../config/dea.yml

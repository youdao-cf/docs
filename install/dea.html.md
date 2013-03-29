
## dependency

on centos 6:

    sudo yum install curl libcurl libcurl-devel

## setup & start

    git clone
    cd dea_ng
    bundle install
    
    # do NOT add '-c' like other component
    bundle exec bin/dea config/dea.yml 

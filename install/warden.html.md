
## dependency

on centos:

    sudo yum groupinstall "Development Tools"
    sudo yum install quota

## config notes

**MUST** modify *config/linux.yml*, make sure you have the lines below to prevent error.

    port:
      pool_start_port: 10000
      pool_size: 10000
      
you can modify other configs to your needs.


## setup

    git clone
    cd warden/warden
    bundle install

    sudo env PATH=$PATH:. bundle exec rake setup[config/linux.yml]

## start 

    sudo env PATH=$PATH:. bundel exec rake warden:start[config/linux.yml]


## dependency

on centos:

    sudo yum groupinstall "Development Tools"
    sudo yum install quota
    sudo yum install glibc-static

## config notes && details

**MUST** modify *config/linux.yml*, make sure you have the lines below to prevent error.

    port:
      pool_start_port: 10000
      pool_size: 10000
      
you can modify other configs to your needs.

## location
Git Repo : https://github.com/youdao-cf/warden
Branch : dev
Commit No: f4cdaba78bae6b6d7aae8f5daa06355775426883

## setup

    git clone https://github.com/youdao-cf/warden.git
    cd warden
	git checkout dev
	git checkout f4cdaba78bae6b6d7aae8f5daa06355775426883
	cd warden
    bundle install

    sudo env PATH=$PATH:. bundle exec rake setup[config/linux.yml]

when the above steps are all done,

**exec “mkdir app” in the path stated in server::container_rootfs_path in config/linux.yml**



## start 

    sudo env PATH=$PATH:. bundel exec rake warden:start[config/linux.yml]


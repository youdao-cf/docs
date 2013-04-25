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


## setup

    git clone
    cd warden/warden
    bundle install

    sudo env PATH=$PATH:. bundle exec rake setup[config/linux.yml]

when the above steps are all done,

**“mkdir app” in the path stated in server::container_rootfs_path in config/linux.yml**

then add the line below in warden/root/linux/skeleton/lib/common.sh

    overlay_directory_in_rootfs /app rw

my post in vcap_dev group shows the reason:

>I'v tried warden on centOS 6.3 for a few days and I found a interesting bug:
>
>since centos do not have aufs or overlayfs in its release, so warden uses the default filesystem - ext4 ,right?
>then it makes path "/" read only and other path e.g. /dev /etc /home /sbin readable and writable.
>
>here is the problem:
i>f I push an app, a dea will try to create a warden container then do "mkdir /app" in it.  app push will definitely report "can not mkdir --read only filesystem"
>but if I change the path "/" to be R&W (by modify root/linux/skeleton/lib/common.sh line 41 right? ) "mkdir /app" will create a dir named "app" in container and in outside path which means in config:container_rootfs_path !!
>so the first app push  seems all right, then restart app , this time the container already has a /app in it , "mkdir /app" will report error: "/app already exist "

>will this problem be fixed in warden? or in dea(no longer create a dir in root path / )?

after this, warden setup is done, BUT you will still have to modify dea_ng source code to make dea_ng working right.

modify line#180 in dea_ng: lib/dea/staging_task.rb 

    -        script = "mkdir /app && touch /app/support_heroku_buildpacks && chown -R vcap:vcap /app"
    +        script = "touch /app/support_heroku_buildpacks && chown -R vcap:vcap /app"

then you may need change the download url presented in buildpack...fuck GFW, slow downloading speed will make the  staging progress timeout...

## start 

    sudo env PATH=$PATH:. bundel exec rake warden:start[config/linux.yml]


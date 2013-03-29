

## why another install doc？

cloudfoundry have two ways to install： 
- vcap（using chef-solo，supporting both virtual machines and physical machines）
- bosh（only support virtual machine）, official recommondation is bosh.

since the official team is focusing on cf 2.0, vcap(we call cf 1.0) is not on active development. 
Thus, it is painful if you have interests on vcap 2.0 but you do not have a required system that bosh needs.

for now, cf does not support other systems except ubuntu 10.04. But technically, all the cf component should work on every linux release.

so we decide to try to install cf manually and individually on our centos 6 system, that's it... 


## prerequisites

- ruby 1.9.3
- bundle


## main component

- nats-server
- warden
- dea
- router
- cloud_controller
- health_manager
- uaa

note： 

- nats-server is the system message mbus and it must install at first
- dea rely on warden in 2.0 so warden must installed before dea.


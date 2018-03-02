# Infrastructure 

If we have a lot of traffic, we may want to split the traffic between two app instances and two database instances. **Load Balancing**

## Infrastructure as Code and Immutability

Two ends of the spectrum.

**Configuration Management** <----------------> **Orchestration Tools**


<---------------------------------------------------------------------------------------->   
Chef, Testing, OAT. Puppet     .Ansible    .````````````Packet. Terrerform 

Tools that are often used on the spectrum. 


**AMI** (Amazon Machine Image)

##Chef

Chef is a configuration management tool. In our classes we will be using Chef, Ansible and Terraform.

**Berksfile** - dependency manager. We will be using metadata.rb since it allows dependies for all files while Berksfile only works for Chef.  
**Kitchen** - configures how we will test the cookbook (platform sections configures environment. We are only using ubuntu so we can delete centos). 

### Kitchen 
-- 
 Kitchen has Three stages.  
 1. Create
 2. Converge
 3. Destroy

 ``` 
 chef generate cookbook nameofcookbook
 kitchen create
 kitchen converge
 kitchen verify
 ```

 Or you can use Test which will spin it up and shut it down. 
 
 recipes folder is where we do the provisioning. 
 
 ```
 package 'name of terminal command'
 ```
 In Test folder, smoke, default_test.rb. Here is where we will write tests to make sure the packages are installed, the server is running as intended etc. 
 


#DevOps - Day 1.
### What is DevOps?

Agile, Lean and DevOps. All tools that can help shorted the Software Development Life Cycle.
Classically takes 2-10 years to create a piece of software.

Prioritises getting a working product out quickly so feedback can come back sooner, and therefore can improve the product faster. Bring all people involved in the life cycle of a product together so that communication is easier, no waiting for replies or getting to the end of a cycle and finding out you have to re do parts. 

###Emerging Requirements

As you are building a product, you may realise there are many more features it needs. Agile allows you to integrate these emerging requirements faster than Waterfall methodology. 

Dev Sec Ops? Adds security into the team. Instead of waiting until the project is finished to test its security, do it during development.

### Four Pillars

1. Ease of Use (Dev)        
2. Flexibility (Dev).        
3. Cost (Ops)	   
4. Resiliency (Ops)


**Monolith** - App and database are on the same machine. 

**Vertical Scaling** - improve the power of your machine. If Monolith, means you’re also upscaling the app unnecessarily. 
Also means that if the machine fails, everything is gone. Monolith = bad


###Working Environement

**Match environment** Match the prodction environment as much as possible.  
**Fast and robust** Make it a pleasure to work with.           
**Easy to update** Keep it up to date so you have access to new features.  
**Support one application at a time** don't let apps share development environments


###Host and Guest
Host is ours and Guest is theirs. 

```bash
vagrant up (creates the environment)
vagrant ssh (places you within the environment)
exit (to get out)
vagrant destroy (to delete the environment)
```
in vagrant 

```bash
sudo apt-get update -y
```

Safe command to run, finds updates and installs them.
Need the -y, trying to automate and the y confirms we want to to install without someone having to type it.

```
sudo apt-get install nginx -y
```

```
curl localhost:80 
```
run inside vagrant

```
vagrant reload
```
run from host
That will attempt to make your changes to virtual host without shutting it down

```
vagrant plugin install vagrant-hostsupdater
```
given us more configuration options we can use inside the vagrant file. 

In vagrant file, 

```ruby 
config.hostsupdater.aliases = ["dev.local"]
```
Add this line of code, allows us to choose the name of the url. 

# Provisional 

## Synced folders 

Links up folders on your host machine to folders on your guest machine. Makes it much easier to write and edit code as you canuse text editors that may be uninstalled on your guest machine. 

zipped app folder. cd into environment, cd into spec_tests and run bundle.

cd into environment folder. 
Create folder called app, create file called provision.sh. 
**provision.sh** is a bash file that allows us to write bash commands

Write bash commands you will/have used in this folder. 

Add ```
  config.vm.provision "shell", path: "environment/app/provision.sh"
  ```
 to your vagrant file to link your provision file.
 

 
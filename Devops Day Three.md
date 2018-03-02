## Production Environments 

### Amazon Web Service (AWS)

AWS is cloud computing. 
Shared Responsibility (They take care of security and hardware, we take care of what goes onto it)

Going to create a virtual box on AWS. 


On AWS, on service, click on EC2. EC2 is similar to the virtual box.
Launch instance. Here he have a list of computer types, similar to how we had different vagrant boxes. Which are essentially pretend laptops.
Choose Ireland as our region, what we have permission for. 
Using the micro one, cheapest for Sparta.  
**Go to configure instance**, change shutdown behaviour to terminate.  
**Add Storage**. Can leave this as it is, 8gb is plenty for what we'll be doing.  
**Add Tag** Add name.  
**Security Group** Choose exiting, choose default. 

```
ssh -i ~/.ssh/IrelandStudents.pem ubuntu@34.240.12.167
```
This open thes virtual machine using the key inside of IrelandStudents.pem

```
chmod 400 IrelandStudents.pem
```
This allows the User of the file to read it, then running the ssh command above we can now access the virtual computer. This is equivalant to the vagrant ssh we used before. 

Permissions are split between Users, Groups and Everyone. Each can have write, read and execute permissions. 

```
rsync -avz -i ~/.ssh/IrelandStudents.pem  . ubuntu@34.240.12.167:/home/ubuntu/app

ssh ubuntu@34.240.12.167
```

This code allows us to take everything from the folder we are in and place it into our virtual machine.

**This is a mutable environment, meaning we can leave it running and make changes to it**

rsyncing the files, npm install, npmjs.

##Middleware 

Two Environments, Dev and Production (UAT)

Multi Machine Vagrant Lab
Multi Machine Vagrancy



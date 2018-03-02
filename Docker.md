#Docker 

Similar to virtual box which creates virtual machines.   
###Containisation

In docker, AMI's are just called images. 

What's nice about docker is that once you have created an image that has been provisioned, if you try and make another image with the same provisions, it won't need to download everything again. 

## Steps

1. Create a dockerfile. This is where we will be putting in our shell commands. Unlike with vagrant, we do not need to sudo these commands. 
2. In terminal, `docker build . -t nameofapp`. This will run the commands and create an image.
3. `docker run nameofapp` This will create a container (virtual box). 
4. 

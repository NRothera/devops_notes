#Chef with Vagrant and AWS


Create Berksfile in your vagrant project folder. 

In here link to your chef cookbooks on github. 

```
source "https://supermarket.chef.io"

cookbook 'mongo', git: "git@github.com:NRothera/chef_mongo_lab.git"

cookbook 'node', git: 'git@github.com:NRothera/Cook_books.git'
```

Run this command to add the cookbooks 

```
berks vendor cookbooks
```

```
wget https://opscode-omnibus-packages.s3.amazonaws.com/ubuntu/12.04/x86_64/chef-server_11.0.10-1.ubuntu.12.04_amd64.deb
```

to install chef

##Knife

```
chef gem install knife-zero
rsync -avz -i ~/.ssh/IrelandStudents.pem . ubuntu@54.246.228.80:/home/ubuntu/app
knife zero bootstrap 54.246.228.80 --ssh-user ubuntu --	node-name app-server
knife node list
knife node show app-server
knife zero converge "name:*" --ssh-user ubuntu
berks vendor cookbook
knife zero converge "name:app-server" --ssh-user ubuntu --override-runlist node

ssh -i IrelandStudents.pem ubuntu@54.246.228.80
# Inside EC2 instance
cd /home/ubuntu/app/app
npm install
node app.js
```

## Packer

uses json to do the commands we had before. We are going to create an Amazon Machine Image which then we can use anywhere. 

```
packer validate packer.json #validates the file
packer build packer.json #will run everything inside the json file
```

```
export DB_HOST=mongodb://54.154.130.79:27017
```
This command links our app to the database. You run this from the app host, so once you've run ssh -i IrelandStudents.pem ubuntu@54.246.228.80. 

## Terraform

Once you've specified if its AWS run

```
terraform init
```

```
terraform plan
```
This checks if there are any errors in the file

```
terraform apply
```
will run the file and create your instance on AWS. 


So we've learnt how to use Jenkins for continuous integration, what tools do you use? 


## CIDR Blocks


octive 










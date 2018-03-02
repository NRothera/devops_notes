# Auto Scaling


Similar to how you create an instance.

Create auto scaling group, choose from you AMI. 
Group size 2, min 2, max 2, instances 2. 

Won't register the two instances you already have as part of the auto scaling group, even though they are part of the AMI. It will create two new instances, and we take down the two original instances (still need to have created two instances originally?).

### If in doubt, put in User Data (bin bash bosh).   



# Sparta Lab Report System Documentantion



## Lab-terraform

### Description

A terraform script for creating a two tiered web application on AWS consisting of an app and a database layer.

## Technologies

* Terraform
* AWS

## Installation and Usage

Clone this repository and substitute the values for ami id in both the app and database sections for your own AMIs. For the app, you can use `packer build packer.json` on a cloned version of the node-app repository to create an AMI ready to be launched into this structure. For the DB layer, use the lab-db repository to produce an AMI for the database layer of the web app and enter this into the db layer of the Terraform file. Once all this is completed enter `terraform init` to initialise Terraform then `terraform plan` for terraform to plan the actions it is going to take then terraform apply to actually launch the app.

## Chef-mongodb-cookbook

### Description

A chef cookbook for installing mongodb on an ubuntu server

## Technologies

* Chef
* Vagrant
* VirtualBox
* Installation and Usage

## Installation of dependancies

* Install VirtualBox from https://www.virtualbox.org/wiki/Downloads
* Install Vagrant from https://www.vagrantup.com/downloads.html
* Install ChefDK from https://downloads.chef.io/chefdk
* Testing the cookbook

Download the repository, navigate to it in the command line and run kitchen test
## Building an AMI

Clone this repository then navigate to it's root in the terminal. Then run berks vendor cookbooks then `packer build packer.json` from the root of a cloned version of this repository to generate an AMI on AWS.

## Sparta Lab App

### Description

This is the app for displaying the data collected by the Jenkins automatic lab marking system

## Technologies

* Node.js

* Request
* Fs
* Ejs
* Express
* Http
* Body-parser
* Mongoose
* Socket.io
* HTML

* CSS

* JavaScript

## Installation and Usage

### To Run Locally

Clone this repository and ensure mongodb is installed on your computer. After the repository is installed, run `npm install` to install all of the Node.js dependancies listed above for the app to run. Then run `export DB_HOST=mongodb://[database ip]` replacing [database ip] with the ip address of your database (most likely localhost if you are running mongodb locally. The app uses this variable to find and connect to the database. Then run `node index` to actually start the app. If any errors occur or the app cannot connect to the database then you will be informed in the console.

To Run On AWS

Clone this repository. You must have packer installed to generate a machine image for deploying to the cloud. Run `packer build packer.json` to create a machine image on aws for launching this app. Then go to the lab-terraform repository, clone it and edit the main.tf file to use the ami name packer generated for your app. Provision a database ami using the lab-db repository and add this to the file also. After all this, run `terraform apply` to get the system up and running on aws.

## Jenkinsfile

### Purpose

This Jenkinsfile determins what happens after a lab has been submitted via a pull request. There are two main outputs:

1. Slack notification containing test results, git author and last commit message.
2. Database output containing test results, name of trainer, name of student and last commit message.
Variables

**output:** The lab test results.  
**author:** The person who made the pull request.    
**message:** The last commit message made on pull request.    
**slackNotificationChannel:** The name of the channel you want slack messages to be sent too.   
**slackStudentId:** The id of the student you want to send slack messages too. This is placed into the readme by the person taking the lab.    
**slackTrainerId:** The id of the trainer you want to send slack messages too. This is placed into the readme by the person taking the lab.    
**js:** 0 if tests are written in Javascript, 1 if tests are written in ruby.    
**recipient:** will contain the student id and trainer id once populateGlobalVariables() function is run.

### Functions

**def isJasmine**   
This test runs Jasmine and places the results into a file. If no jasmine tests are found, then it adds one to the variable js. During the build, the variable js is checked. If it equals 0, the output will equal the results of the Jasmine test. Anything other than 0, then rspec will be run and the output will equal the output the results.

**def notifySlack(text, recipient, attachments**   
This function sends information to slack in the form of an attachment. It takes in three parameters, text, recipient and attachments. Text will always be equal to empty string (e.g. "") as the attachments are placed inside.
The variable slackUrl can be changed to equal the webhook of whichever slack channel you want the information to be send too.
The variable payload is where the important information is placed. Channel will decide who receives the information, attachments contain the test results, branch name, author name and last commit message.

**def databaseInfo(studentName, teacherName, score, labName).**       
This function takes in four parameters. These paramters are what will be sent to the database and displayed on the web app.

**def getGitAuthor.**   
This function gets the name of the person who submits the pull request.

**def getLastCommitmessage.**   
This function gets the last commit message of the person who made the pull request.

**def populateGlobalVariables.**    
Runs the functions and above and populates the variabels. It also grabs the ids from the readmes and trims away any whitespace.

# node build

 Machine Currently the build run on **ALMS**. This machine has been provisioned to run Ruby and Jasmine tests, however you can change this to whatever machine you have created to fit your needs.

 Checkout Takes the files from the Github repo from the pull request and places them onto the machine.

Build Determins whether to output ruby results or jasmine results based on the variable js. The results are outputted into a file and only the results are taken out using regular expression.
The test results, the name of the Github repo author, their last commit message and the pull request number are then sent to the appropriate people on slack.

The name of the person who submitted the pull request, the trainers name, the test results and their last commit message are also then send to the database through a shell command. `databaseInfo("${author}", "Steve", "${output}", "${message}" )`

# Lab Repo - Include 2 Pushes

### What is the 2 push limit?

Git has functionality to execute commands upon the user carrying out specific actions. We have the ability to manipulate what happens when these actions are carried out. For example, git has a hidden folder called `.git/hooks`. In this folder, there are bash scripts that run at specific events. The name of the file indicates the stage of their execution i.e. pre-commit will run before a user commits. To limit the number of pushes the `pre-push` file can be altered.

### Setup

#### Creating a Pre-push file

A pre-push file needs to be created. Navigate to the lab repo and,

	mkdir .githooks

	# Creates the pre-push and add functionality, gets generated in the .git/hooks directory
	npm install --save-dev pre-push

	chmod +x .git/hooks/pre-push
	mv .git/hooks .githooks
>NOTE: The pre-push file must be outside of the .git folder as that folder is only for your local machine. However, the file itself will only work when it is in the .git/hooks directory. This is solved by using a NPM script which will be explained further on.
### Writing the script

Add the following code to the top of the file, just under the `#!/bin/bash` line. These are the variables setting up our next task of creating the limits.

	# Max number of pushes
	MAX_NUMBER=2

	# Reads in from this text files
	BASE_NUM="$(cat ../textfile.txt)"

	# Converts a string into an integer
	NUM="$(($BASE_NUM+0))"
To prompt the user to confirm whether or not they would like to push. Add this code to the bottom of the pre-push.

	read -p "You're about to push, are you sure you won't break the build? " -n 1 -r < /dev/tty
	echo
	if [[ $REPLY =~ ^[Yy]$ && $NUM -lt $MAX_NUMBER ]]; then
	    echo You are now pushing...
	    echo $BASE_NUM
	    echo
	    let BASE_NUM=BASE_NUM+1
	    echo $BASE_NUM > ../textfile.txt
	    exit 0 # push will execute
	else
	    read -p"You cannot push..." -n 1 -r < /dev/tty
	    echo
	    exit 1 # push will not execute
	fi
This will prompt the user to confirm their push. Textfiles to store the number of pushes will be created if non-existant. Otherwise, the number inside will be incremented on push.

### NPM script

In the package.json file, there is a object for adding custom scripts. Change the file to reflect the following:

```

"scripts": {
    "test": "eslint .",
    "spartalab": "mv ./.githooks/pre-push ./.git/hooks && npm install && git config --global user.name"
	},

```

When the user types in npm run spartalab <GITHUB_USERNAME> the pre-push file will be moved to the correct place and the user will have their github userame added to the global git configuration (for slack messages).

## Multi-labs

**This README assumes that there are multiple labs inside the organization. Created a personal access token so that you can create a Jenkins credientials to access the GitHub through GitHub API.**

1. Create a New Item in Jenkins

2. Give the New Item a name and choose it as a GitHub Organization

3. Click on the Configure option for the project created

4. Under projects section, create or choose a Credentials to access the GitHub for scanning branches and pull requests. Only "username with password" credentials are supported where password is the GitHub personal access token.

5. Owner is the organization name

6. Under Behaviours:

 >Discover branches

	* Strategy - Only branches that are also filled as PRs

 >Discover pull requests from origin

	* Strategy - The current pull request revision

 >Discover pull request from forks

	* Strategy - The current pull request revision
	* Trust - Everyone.  

7. Under Project Recognizers:

	* Pipeline Jenkinsfile

	* Give /path/to/the/Jenkinsfile.
8. Scan Organization Triggers

	* Check Peridically if not otherwise run box
	* Interval - 1 day

9. Under Automatic branch project triggering:

 >Branch names to build automatically

 * Put PR-\d+ if you want to build any branch with pull requests. or name of the lab you want to build using regular expression.

10. Apply and save the configuration.

11. Scan organization to match the labs matching the regular expression applied on step 9.

# Jenkinsfile

## Purpose

This Jenkinsfile determins what happens after a lab has been submitted via a pull request. There are two main outputs:   
1. Slack notification containing test results, git author and last commit message.   
2. Database output containing test results, name of trainer, name of student and last commit message.  


## Variables

**output:** The lab test results.  
**author:** The person who made the pull request.   
**message:** The last commit message made on pull request.  
**slackNotificationChannel:** The name of the channel you want slack messages to be sent too.  
**slackStudentId:** The id of the student you want to send slack messages too. This is placed into the readme by the person taking the lab.   
**slackTrainerId:** The id of the trainer you want to send slack messages too. This is placed into the readme by the person taking the lab.   
**js:** 0 if tests are written in Javascript, 1 if tests are written in ruby.    
**recipient:** will contain the student id and trainer id once populateGlobalVariables() function is run.

## Functions

**def isJasmine**   
This test runs Jasmine and places the results into a file. If no jasmine tests are found, then it adds one to the variable **js**. During the build, the variable **js** is checked. If it equals 0, the output will equal the results of the Jasmine test. Anything other than 0, then rspec will be run and the output will equal the output the results.   

**def notifySlack(text, recipient, attachments**   
This function sends information to slack in the form of an attachment. It takes in three parameters, text, recipient and attachments. Text will always be equal to empty string (e.g. "") as the attachments are placed inside.   
The variable **slackUrl** can be changed to equal the webhook of whichever slack channel you want the information to be send too.  
The variable **payload** is where the important information is placed. Channel will decide who receives the information, attachments contain the test results, branch name, author name and last commit message.   

**def databaseInfo(studentName, teacherName, score, labName)**.  
This function takes in four parameters. These paramters are what will be sent to the database and displayed on the web app.

**def getGitAuthor**.  
This function gets the name of the person who submits the pull request.

**def getLastCommitmessage**.   
This function gets the last commit message of the person who made the pull request. 

**def populateGlobalVariables**.  
Runs the functions and above and populates the variabels. It also grabs the ids from the readmes and trims away any whitespace. 


##node build

###Machine
Currently the build run on **ALMS**. This machine has been provisioned to run Ruby and Jasmine tests, however you can change this to whatever machine you have created to fit your needs. 

###Checkout
Takes the files from the Github repo from the pull request and places them onto the machine.   

###Build
Determins whether to output ruby results or jasmine results based on the variable **js**. The results are outputted into a file and only the results are taken out using regular expression.   
The test results, the name of the Github repo author, their last commit message and the pull request number are then sent to the appropriate people on slack. For example

![Example](/Users/tech-a48/Desktop/Screen Shot 2018-02-26 at 15.35.00.png)

The name of the person who submitted the pull request, the trainers name, the test results and their last commit message are also then send to the database through a shell command. 
`databaseInfo("${author}", "Steve", "${output}", "${message}" )`






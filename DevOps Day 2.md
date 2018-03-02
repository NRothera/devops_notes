#DevOps - Day 2.


# SSH basics

PSK = Pre-shared Key where the key is shared between two parties before the secure thing is shared.

Public key is always shared. Like a padlock, it allows access to a software when the private key is correct.

Therefore, the private key does not need to be shared since you pass the "padlock", the public key to the authentication service. The authentication service then "attaches the padlock" to the software and only you have the correct key to open this "padlock". The public and private keys are linked by a mathematical formula that only your computer knows.

## Public and private keys
The keys on your computer are kept in the `~/.ssh` folder. We use a program called ssh-keygen to generate keys. It will ask some questions as you go through the process.

```bash
cd ~/.ssh
ssh-keygen -t rsa -b 4096 -C nrothera@spartaglobal.com"
```
Leave the password empty to allow automation to go through.

## Jenkins

Configure file. Need Github url and git clone url. 

Github webhook (sparta url is http://jenkins.spartaglobal.academy:8080/github-webhook/)
This links jenkins to Github, now when we push code to master it will run the tests. If a test fails, it won't be synced to master. 

In Jenkins configure, build, executable shell
cd app
npm install
npm run test-unit
npm run test-integration

npm runs the unit and integration tests that have been written. 



## Principle of least privilege

***Don't give anyone more permissions than they need to do the jobs they need to do.***

>
[Example - Sparta laptops are not adhering to the principle of least privilege since students are able to do a lot of things on it. So it's somewhat more difficult to adhere to it since it's so easy to give lots of permissions in one go.]

>
[Example - Sparta's AWS account restricts what students can do greatly and does adhere to the priciple of least privilege.]

So we start with no permissions and add permissions until it's enough rather than starting with all permissions and taking them away until it's secure.

Server hardening == locking down everything in case someone does manage to get into the machine.

**What can we access currently that doesn't need to be accessed?** --> the database.

## Subnets
VPC contains all our machines on it. The default VPC has one subnet, ie a sub network. We use subnets to separate machines that don't need to see each other. 

_Public subnets_ can be seen from the internet and our single subnet is public at the moment. However, we don't want the database to be visible from the outside world but it still needs to talk to the app which will be on a public subnet.

## IP addresses
An IP address is split into octets and they're called octets because they are in 8 bits.

### Binary
If you want to represent something in decimal, we need 10 different digits: 1, 2, 3, 4, 5, 6, 7, 8, 9, 0. In binary, we only use two: 0 and 1. 

Back to decimal, the columns go up in powers of 10, eg. units, tens, hundreds, thousands... etc. But in binary, the columns go up in powers of 2: 2, 4, 8, 16, 32... etc. and all numbers can still be represented like this.

| Decimal | 64 | 32 | 16 | 8 | 4 | 2 | 1 |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 5 | - | - | - | - | 1 | 0 | 1 |
| 27 | - | - | 1 | 1 | 0 | 1 | 1 |
| 73 | 1 | 0 | 0 | 1 | 0 | 0 | 1 |

The highest number that can be represented by 8 bits is 255 so the "highest" ip address you can have is 255.255.255.255. From this, the maximum number of permutations is around 4.2 billion. But there are many internet connected devices per person in the world and there are 8 billion people in the world. That implies that there won't be enough ip addresses for each device.

## Subnet mask

Subnet mask is how we determine if a computer is in our network or not. It looks at the binary version of the ip address and anywhere it sees a 1 on the first 3 octets of our network, it will check that the other ip also has a 1.

**Does not match**

11000000.10101000.00001010.10010110

11111111.11111111.11111111.00000000

**Does match**

11000000.10101000.00001010.10010110

10111111.11111111.11111111.00000000

If the machine is on our network, then the computer will know where to send the traffic. If the machine is not on our network, then it will send the traffic to the router which can then decide where it should go via a route table.

***NAT - Network Address Translation*** - 
Translates ip addresses from the internet to where it should go when a message is received. 

## CIDR blocks
CIDR format is a way of writing an IP address that specifies how much of it should be matched. This is done by adding a `/[number of bits]` eg. 10.0.0.0/16 means that the network includes the first 16 bits, ie two octets.

## VPC (Virtual Private Cloud)

***Route table*** - a table of ip addresses and where info from each should go to.

***Internet Gateway*** - Is essentially a route table for the internet? It's a thing that lets you redirect the traffic to the internet if you can't find it on your route table.

1. Create a VPC 
2. Add subnets in that VPC
3. Create a route table
4. Create Internet Gateway
5. Add a public route into the route table at the destination 0.0.0.0/0 (the /0 is the important bit as it says don't match anything)
6. Create another two EC2 instances (one for the app and one for the db) and add them to the new VPC.
7. ssh into the instance
8. Go to Security Groups and set the VPC to default on the app
9. Then go back to the instances page, click your VPC and click Set security group
10. Set the security group to My IP for the database.

## Security groups
Our network has a layer of security that doesn't allow inbound traffic if it doesn't come from the right place. This only looks at ip address though so we can add more layers of security via security groups which will match ip, protocol and port.

1. Click Create security group
2. Click Add Rule and select HTTP. It will default to TCP protocol and port 80. The other protocol available would be UDP which is faster than TCP but we're not going to use it because it's not common.
3. Change the Source to My IP. Notice the 
4. Add another rule and select SSH and change the source to My IP as well.

>
If the conection times out, it's because of the firewall blocking it. So you need to go to Security groups and add your ssh.

### Security groups are stateful.
The outbound rules of the security group are in reference to the requests that come from our database outwards. But and incoming requests will require a response and these will always be allowed.

## NACL (Network ACLs)
These are stateless, unlike security groups, so you have to create BOTH inbound and outbound rules. These rules run from top to bottom in that order so if the top rule is allow everything then it will never get to rules that are lower down.

So if we want to only allow access from one IP address, replace the allow all rule with the specific IP address. For our app, we'd want to also add an ssh to it and for the database, we want to deny all inbound traffic. When adding a rule, the rule number determines what order the rules are displayed and adhered to in ascending order.

Note that security groups are attached instances and network ACLs are attached to subnets.

configure instance details 
create new security group call it bastion (description ports for Bastion server)
Source My IP
Choose existing key pair (Ireland Students)
Don't need ssh access to app instance anymore now that bastion server is running
Change inbound rules to only allow access to bastion server
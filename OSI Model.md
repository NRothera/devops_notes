## OSI Model (Open Systems Interconection)

Seven layers to the model. **Debugging**.  
1. **Physical layer** (cables, the computer, routers). ***Bits***
2. **Data Link Layer**. Splits the bits up into frames. Handling the physical identification of hardware using MAC addresses.   ***Frames**.  
3.**Network layer** (routing, route tables). When data arrives at the network layer, the source ip and destination ip are examined, port.  It decodes the bits from the first layer. It will address an ip address to a MAC address. An ip is a logical destination (it isn't physical, otherwise it wouldn't be able to change). ***Source IP, Destination IP***.   
4. **Transport layer**. TCP, UDP. Unit Directional Protocol (e.g. used for streaming in games. Just sends data out fast as possible not worrying about order. Ping!).  ***Segments***. Makes sure want is sent is received, error detection.   
5. **Session Layer**. Maintain connection between source and destination. ***Data***.  
6. **Presentation Layer**. Taking those bits and bringing them together into the things they should be. Handing HTTP, JPEG, GIF. This is built into the browser, when they get a package they know how to deal with it. **Decryption and Encryption** happens here.  ***Data***.   
7. **Application Layer**. First point where you have access to everything. All the information that was available at the source has now be debugged and decoded and is now available here. **Data**.      
##Risk Registers
####Description
If the world is destroyed by an asteroid.  
Internet failure at server in one region.   
Server overload (lots of people accessing at once).   
Database goes down

####Probability
Very low.   
Very low.   
High.   
High. 

####Impact
High impact.  
High impact.   
High impact.   
High impact.

####Options
Mars Server.   
Multi Availability Zone.   
Load balancing, autoscaling, vertical scaling.  
Backups, confirmation message, replication (master server and slave server), Load balancing


####Solution/Action to solve
No solution as of yet. 

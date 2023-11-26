# Blackwing Protocol

Blackwing's main goal is to provide small devices with the capability of connecting to multiple services in a end-to-end encrypted channel. 
The main idea is that the devices have to connect to a single point in the network. Then, they can access any service that run under a local network, for example.
This way, one has to secure a single point, which is easier than securing multiple points. 
Moreover, no one in the network can precisely know which service a device is calling. For example, imagine you are updating a MongoDB database, whose default port is 27017. 
By checking for upcomming traffic at this port, any malicious player can know that you are connecting to a MongoDB database. However, under a Blackwing network, no one can precisely know that you are connecting to a MongoDB. 

![alt text](figs/bwarch.svg)

## Default Wire Protocol

![alt text](figs/mesage.svg)

## Sesion Wire Protocol

![alt text](figs/session_wire.svg)
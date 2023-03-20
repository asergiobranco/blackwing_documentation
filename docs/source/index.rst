Blackwing Protocol
==================

Blackwing protocol's design and implementation started five years ago. The main idea was to design a secure protocol that worked as a method to create distributed networks and systems that worked across multiple machines and programs, called microservices.  

The reason for such interest was that with it, one could quickly deploy a novel application without opening a new port or worrying about complex setups. A single server would receive data from any client, decrypt it, and send it to the corresponding microservice without worrying about what and how it worked. This way, the systems were independent; even the same microservice could be accessed through multiple ids or separate servers. 

The protocol should provide an end-to-end encryption channel and make no assumptions regarding the microservice's job or algorithm. It should be secure but smaller enough for a Resource-Scarce Embedded System  (RSES). It should focus on allowing a network to be built as the developer whished and be versatile enough to the goal that the Cloud should be the one shaping itself to the device, not the other way around. 

Therefore, Blackwing was born as an encapsulation protocol, that in some extent, worked similarly to a VPN connection. The client only needed a single server to be accessed to communicate with billions of smaller and independent microservice that performed a single task. In the middle, no one could know which microservice was being accessed, and no one could know what was being sent. 

Encryption was a priority, and the RSA asymmetric encryption and AES-128/AES-256 were used as a building block for the protocol. The client uses the server's public key to encrypt a message called STAMP. The STAMP is where the client indicates the microservice's ID it wants to communicate with, the AES Key and Initial Vector, and some other information. 

The stamp needed to be large enough to hold this information but smaller enough to reduce communication overhead. Because the RSA key's size determines the number of bytes a message has after encryption and encrypts in minimum blocks, the best solution was that the RSA key could not be smaller than 1024 bits. However, larger keys should be used whenever possible because they take longer to break. 

A 1024-bit RSA key generates a 128 bytes STAMP, which is the minimum overhead the protocol imposes. If one uses larger keys, then the size doubles as the key doubles. However, some experiments and lack of support by some RSES platforms regarding encryption methods have raised the need for an extra unencrypted byte, named MASK. This extra byte provided a much more exciting solution to reduce traffic, as will be explained shortly. Thus, the Blackwing protocol's total overhead is 129 bytes (for a 1024-bit key). 

But the STAMP and MASK regard the server. What about the microservice? The microservice will receive what I came to call a LETTER. The LETTER is encrypted using the AES key and IV set in the STAMP. There are multiple AES modes, but Blackwing accepts only two: the CFB (preferred) and the CBC. The AES mode is defined through a bit present in the MASK. As long as the connection is kept open, all traffic that passes between the client and the server, and vice-versa, is encrypted with the aES context set on by the STAMP.


Contents
--------

.. toctree::
   :maxdepth: 3
   
   protocol
      mask
      stamp
      letter
   stamp
   session
   usage
   api

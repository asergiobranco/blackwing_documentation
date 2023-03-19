Protocol
=====

Blackwing protocol's design and implementation started five years ago. The main idea was to design a secure protocol that worked as a method to create distributed networks and systems that worked across multiple machines and programs, called microservices.  

The reason for such interest was that with it, one could quickly deploy a novel application without opening a new port or worrying about complex setups. A single server would receive data from any client, decrypt it, and send it to the corresponding microservice without worrying about what and how it worked. This way, the systems were independent; even the same microservice could be accessed through multiple ids or separate servers. 

The protocol should provide an end-to-end encryption channel and make no assumptions regarding the microservice's job or algorithm. It should be secure but smaller enough for a Resource-Scarce Embedded System  (RSES). It should focus on allowing a network to be built as the developer whished and be versatile enough to the goal that the Cloud should be the one shaping itself to the device, not the other way around. 

Therefore, Blackwing was born as an encapsulation protocol, that in some extent, worked similarly to a VPN connection. The client only needed a single server to be accessed to communicate with billions of smaller and independent microservice that performed a single task. In the middle, no one could know which microservice was being accessed, and no one could know what was being sent. 

Encryption was a priority, and the RSA asymmetric encryption and AES-128/AES-256 were used as a building block for the protocol. The client uses the server's public key to encrypt a message called STAMP. The STAMP is where the client indicates the microservice's ID it wants to communicate with, the AES Key and Initial Vector, and some other information. 

The stamp needed to be large enough to hold this information but smaller enough to reduce communication overhead. Because the RSA key's size determines the number of bytes a message has after encryption and encrypts in minimum blocks, the best solution was that the RSA key could not be smaller than 1024 bits. However, larger keys should be used whenever possible because they take longer to break. 

A 1024-bit RSA key generates a 128 bytes STAMP, which is the minimum overhead the protocol imposes. If one uses larger keys, then the size doubles as the key doubles. However, some experiments and lack of support by some RSES platforms regarding encryption methods have raised the need for an extra unencrypted byte, named MASK. This extra byte provided a much more exciting solution to reduce traffic, as will be explained shortly. Thus, the Blackwing protocol's total overhead is 129 bytes (for a 1024-bit key). 

But the STAMP and MASK regard the server. What about the microservice? The microservice will receive what I came to call a LETTER. The LETTER is encrypted using the AES key and IV set in the STAMP. There are multiple AES modes, but Blackwing accepts only two: the CFB (preferred) and the CBC. The AES mode is defined through a bit present in the MASK. As long as the connection is kept open, all traffic that passes between the client and the server, and vice-versa, is encrypted with the aES context set on by the STAMP. 

.. _Mask:

Mask
------------

The first byte of a Blackwing message is the setup for everything that comes next. This byte is called the mask and provides the server with information regarding what to expect and how to behave. 

The first bit (LSB) in the mask defines if the message is sent in ``secure (if 1)`` or ``not secure (if 0)`` mode.

.. note::
  
   The mask is what defines the server's behaviour. Therefore, pay attention to what you are setting. Most errors occur due to a bit mistake. 
   
.. drawio-figure:: source/example.drawio
   :format: png


.. _SecureMode:

Secure Mode
~~~~~~~~

The secure mode tells the server that the Stamp is encrypted with the RSA public key. This mode 

.. _NotSecureMode:

Not Secure Mode
~~~~~~~~

The not secure mode does not means that the message sent is not encrypted or that it does not go through an encrypted channel. It is just a way for the server to know that the Stamp is not encrypted through RSA. This mode should be used for:

* Request Public Key.
* Use previous stored session.
* To simply send a message to the microservice without any encryption (optional).


  
.. _Stamp:

Stamp
------------

The Stamp provides the server with information regarding the microservice to foward the message, and the AES keys to use. The Stamp is composed of 5 fields, one of them being optional, but should be considered to **avoid stamp reutilization**, which can be one method of attack. The stamp must be encrypted with the RSA public key. 


The fields in secure mode are:

#. Microservice ID (64 bits)
#. AES Key (16 bytes)
#. AES IV (16 bytes)
#. Stamp Request (1 Byte)
#. Timestamp (optional)


It exists a special Stamp for mask's ``not secure`` mode. This is when the client tells the server that there it is using a previous stored session. In this case, the Stamp is 8 bytes which represent the Session ID. For this case scenario, the AES Keys and IV to use are the previous ones. The server will send the message to the Microservice to whom the client connected before. 



.. note::

  The Stamp's size depends on the RSA key size chosen. For a 1024-bit, the minimum acceptable, the Stamp's size is 128 bytes. The size doubles as the key doubles in size.

.. note::

  The stamp must be msgpack serialized.
  
.. _Letter:

Letter
------------

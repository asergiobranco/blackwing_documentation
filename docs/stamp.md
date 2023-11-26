The Stamp tells the server how the client has encripted the letter, and which microservice it wants to connect with. The stamp mut be encrypted using one of the available RSA methods. 
The stamp is an array with the minimum of 4 elements, serialized with `msgpack`. 

The first element is the `Microservice ID` which is composed of 8 bytes (uint64_t). 

The second and third fields are the `AES Key` and `AES IV` respectively. The IV must be 16 bytes, however the key size depends on whether you want to use AES-128 ou AES-256. If you want to use AES-128, the key must be 16 bytes, otherwise use a 32 byte key. 

The fourth element is the `smask`, a single byte which allows to ask the server for multiple things, one of which is the session. 

A fifth element may be present, which is the `timestamp`. This tells the server when the stamp was generated. For networks where the Public key is only known by the devices, helps the server to verify whether it may be receiving trafic from a malicious player, since one of the attack vectors, could be someone reusing a previous generated stamp to make a DoS. 

Because one can choose the RSA key size, and the minimum is 1024 bytes, if one wants to use the timestamp, it is advised that the AES Key to be 16 bytes.

![alt text](figs/stamp.svg)

The following figurs show how the message is packed with msgpack. Someone that does not intend to use one of its standard libraries, can simply create a byte array that respect these formats. 

![alt text](figs/stamp_msgpack.svg)

![alt text](figs/stamp_msgpack_no_tms.svg)
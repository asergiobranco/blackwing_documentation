# Session

Blackwing sessions are the way to avoid the overhead imposed by the protocol. When you create a session, the server stores the AES Key, AES IV, and the Microservice ID sent in the stamp when the session request occured. The server will respond to the request with 16 bytes, sent before the microservice response. The 16 bytes are AES encrypted, and should be decrypted before storing it. If the 16 bytes are all `0`, then the server could not create a session. Otherwise, the session was created.

The first 8 bytes are the `Session ID` that should be send in the upcomming messages. The next 8 bytes are the timestamp, representing the Time to Live of such session, therefore the time when the session will expire. 

![alt text](figs/session_response.svg)

To request a session, the `smask` should have its LSB set to `1`. As soon as this bit is set to `1` the upcomming 16 bytes of the response are a session. Please, ensure that this bit is `0` if you do not want to receive a session, otherwise you may get the "wrong" response from the microservice.

![alt text](figs/request_session.svg)

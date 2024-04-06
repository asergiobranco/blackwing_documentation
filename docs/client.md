# Clients

The available frameworks, packages, and tools to easily communicate with any blackwing server will be presented in the following page. 

## Python Client

The Blackwing Project provides simple python implementation for a Blackwing Client. The code is available at [https://github.com/asergiobranco/blackwing-client](https://github.com/asergiobranco/blackwing-client).

The blackwing client allows anyone to easily send messages through a blackwing server to any microservice.
The client is not microservice oriented. Therefore, you can create a single client for a server, but use several microservices.

The client is capable of keeping contexts. Therefore, from message to message you will have access to previous stamps and sessions.



### Example

```python
from bwclient.client import BwClient

client = BwClient(
    PUBLIC_KEY, # The public key
    "localhost", #the server hostname 
    8000, # the bw server port number
    "OAEP", # the RSA algo
    "SHA1", # the sha algo 
    "CFB", # the AES mode
    128 # The segmentsize for CFB
)

client.set_microservice(
    0xff, # microservice id to connect
    True # if one should ask for a session
) 

client.send_to_ms(
    '0xff', # microservice id to send
    b'Hello World' # the message
)

print(
    client.reacv_from_ms(
        0xff # microservice id to receive
    )
)

client.close_ms(0xff)
```

### API

`set_default_context(self, rsa_type, rsa_sha, aes_type, aes_segsize)`

    > Creates a default setting for any microservice. It basically tells which configurations to use for the stamp.
    > rsa_type : str
    >   Should be a str either "OAEP" or "v1.5". 
    >   You should use the OAEP by default, since is the safest.
    > rsa_sha : str
    >   Should be either "SHA1" or "SHA256".
    > aes_type : str
    >   Should be either "CFB" or "GCM"
    > aes_segsize : int
    >   Should be a multiple of 8. 
    >   min value: 8 - max value: 128

`set_microservice(self, microservice_id : int, request_session : bool)`

> Creates a microservice context, that can be accessed through microservice_id
> microservice_id : int or str
>   An integer or hexadecimal string that should be equal to the microserice_id in the server.
> request_session : bool
>   Wheter or not to request a session in the first connection. 
>   If True, the client will request a new session in every new connection, if the server fails to provide a valid session in the previous requests.

`send_to_ms(self, microservice_id : int, msg : bytes)`

`recv_from_ms(self, microservice_id : int)`

`close_ms(self, microservice_id : int)`

`destroy_ms(self, microservice_id : int)`

`request_for_new_session(self, microservice_id : int)`

## Browser Client

Because some microservices may be HTTP/Web applications, and no browser yet provides mechanisms to translate from HTTP into Blackwing, a browser is provided.
With this browser you can develop and share your 

## Blackwing Tunnel

# BlackWing Microservice

The blackwing microservice framework was designed to help anyone develope their own microservices routines, without much trouble.
This framework is a standard python implementation, that has a TCP/IP server engine and uses msgpack serialization in order to receive and send data to the user.
The engine is capable of receiving data either in a open->receive->process->send->close way, or in a streaminng version.
The framework is currently available in a docker fashion, but sooner the code will be available.

# Configuration File

The microservice must receive a configuration file, whose path must be passed in the `BlackwingMicroservice` class constructor. 
The `hostname` and `port` tell the microservice if the traffic can be accepted or not.
The `backlog` regard the maximum number of clients the microservice can handle at the same time.
The `timeout` is the maximum amount of seconds the microservice will keep the socket open if no traffic passes over it.  

```yaml
hostname: 0.0.0.0
backlog: 10
port: 5000
timeout: 10
```

# How to create a Microservice

Creating microservices is quite simple, you just have to define the handler to use, and the class to the `BlackwingMicroservice(configuration_path="/path/to/config.yaml", handler=MyHandler)`. You will receive an error such as:

```
Traceback (most recent call last):
  File "/usr/local/lib/python3.11/site-packages/BlackWing_Microservice-0.4.0-py3.11.egg/bwmicroservice/handler.py", line 64, in __del__
AttributeError: 'NoneType' object has no attribute 'close'
```

This is normal, because the constructor tests the `MyHadler` to check if he has all the necessary attributes and routines. 

## One-shot Microservice

A one-shot microservice is a microservice that will receive a single message, and send a response to the client, and then close the connection
This is the simplest kind of client-microservice behaviour that can exist, and many applications fall into this design.
To create a one-shot microservice you must create a file such as the following:

```python
from bwmicroservice import BlackwingHandler
from bwmicroservice import BlackwingMicroservice

class MyHandler(BlackwingHandler):
    def setup(self):
        print("I will before receiving anything")

    def attendRequest(self):
        print(self.letter)
        self.response = self.letter

microservice = BlackwingMicroservice(configuration_path="/path/to/config.yaml", handler=MyHandler)
microservice.start()
```

In this example you are creating a microservice that will echo what the client has sent back to it. 
You will notice the class `MyHandler`, and that it has two routines set: `setup()` and `attendRequest()`.
The `setup()` routine is what the microservice must run before even receive the client message. 
The `attendRequest()` is the routine to run when you receive the letter from the client. 
When the `attendRequest()` routine is called, you will have available the attribute `self.letter`
which holds the message sent by the client.
Whatever you want to send to the client, use the `self.response` attribute. You do not have to 
`msgpack` it, it will be done in the background before sending it. 

## Streaming Microservice

If the client or the microservice will process the request and send multiple message, you can use the 
streaming handler. In this case, you should use this example:

```python
from bwmicroservice import BlackwingHandlerStreamer
from bwmicroservice import BlackwingMicroservice

class MyHandler(BlackwingHandlerStreamer):
    def setup(self):
        print("I will before receiving anything")

    def attendRequest(self):
        print(self.letter)
        self.response = self.letter

    def clean(self):
        print("I will run this after the last message is sent/received")

microservice = BlackwingMicroservice(configuration_path="/home/bwmicroservice/msdir/config.yaml", handler=MyHandler)
microservice.start()
```

# Docker

The docker container contains the directory `/home/bwmicroservice/msdir/`. This directory, ideally should contain four files to init the microservice routine: `config.yaml`, `main.py`, `requirements.txt`, `setup.sh`.

The `config.yaml` file should be similar to the following example:

```yaml
hostname: 0.0.0.0
backlog: 10
port: 5000
timeout: 10
```

The `hostname` and `port` tell the microservice if the traffic can be accepted or not.
The `backlog` regard the maximum number of clients the microservice can handle at the same time.
The `timeout` is the maximum amount of seconds the microservice will keep the socket open if no traffic passes over it. 

The `main.py` should be something like this:

```python

from bwmicroservice import BlackwingHandler
from bwmicroservice import BlackwingMicroservice

# You can directly create the handler in this file
# but usually is better to create your own library
from mylibrary import MyHandler

microservice = BlackwingMicroservice(configuration_path="/home/bwmicroservice/msdir/config.yaml", handler=MyHandler)
microservice.start()
```

The `requirements.txt` should contain all the python packages and respective versions to be downloaded and installed by pip.

The `setup.sh` file is run everytime before the microservice is started, it can be used to install the libraries or to make changes in the file system. This allows you to modify the container, without having to rebuild it.

Another option for you, is to build your own container from the base image provided.

## Run the container

`docker run -d -p 5000:5000 $(PWD):/home/bwmicroservice/msdir/ asergiobranco/blackwingmicroservice`

## Example

You can find an example for creating a microservice in [https://github.com/asergiobranco/blackwing-microservice-example](https://github.com/asergiobranco/blackwing-microservice-example).
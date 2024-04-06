# Docker

There are two docker images currently available for anyone to use. The docker images are under the **GNU GPLv3 licence**. 
The docker images provide an easy way to run a Blackwing Server or create a Microservice. However, remember that any service
can be a microservice, and you do not have to specifically use the Microservice image. 

## Blackwing Server

The Blackwing Server image can be downloaded using the following command.
By default, this last image uses the python:alpine linux as its base. I have faced some problems
when using this image with a number of clients higher than **100**. But, for many applications, 
this number is enough. 

`docker pull asergiobranco/blackwingserver`

To run the server you must do the following:

`docker run -v $(pwd):/home/bwserver/serverdir -p 5000:5000 asergiobranco/blackwingserver`

In this case, if no `server.yaml` files is found in the current directory `$(pwd)`, then it will create a new one from scratch 
using the default settings. You must then create `microservices` folder in the `$(pwd)` to add microservice files to route.

### Configuration file

If a configuration file `server.yaml` exists inside the volume, the server will start with those configurations. Otherwise, it will automatically generate a cofiguration file with default settings, and a random RSA key. 

You can access the public key by reading the logs. The server, when started, will output the RSA public key inn different formats. 

### Available tags

There are currently four available tags:

* asergiobranco/blackwingserver:lastest
* asergiobranco/blackwingserver:alpine
* asergiobranco/blackwingserver:slim
* asergiobranco/blackwingserver:bookworm

The alpine and latest are the same.

### Getting the RSA public key

You can access the public key by reading the logs. The server, when started, will output the RSA public key inn different formats.

`docker log <CONTAINER_ID>`

Another solution is to load the yaml file and use the pytho library to export the public key.

## Blackwing Microservice

The blackwing microservice docker image allows one to easily run microservice using the framework originally developed 
for blackwing. In this case, the routines must be written in python to work.

The underlying framework licence is the **GNU GPLv3 licence**, which means that you must provide you microservices 
routine code in open-source. 

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

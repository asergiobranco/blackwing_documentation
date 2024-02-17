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


### Available tags

There are currently four available tags:

* asergiobranco/blackwingserver:lastest
* asergiobranco/blackwingserver:alpine
* asergiobranco/blackwingserver:slim
* asergiobranco/blackwingserver:bookworm

The alpine and latest are the same.



## Blackwing Microservice

The blackwing microservice docker image allows one to easily run microservice using the framework originally developed 
for blackwing. In this case, the routines must be written in python to work.

The underlying framework licence is the **GNU GPLv3 licence**, which means that you must provide you microservices 
routine code in open-source. 

`docker pull asergiobranco/blackwingmicroservice`


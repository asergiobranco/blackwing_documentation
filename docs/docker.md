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

### Available tags

There are currently four available tags:

* asergiobranco/blackwingserver:lastest
* asergiobranco/blackwingserver:alpine
* asergiobranco/blackwingserver:slim
* asergiobranco/blackwingserver:bookworm

The alpine and latest are the same.


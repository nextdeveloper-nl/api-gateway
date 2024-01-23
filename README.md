# NextDeveloper API Gateway
Omni channel events service that will collect and redirect the related messages through various different channels and in between applications. This service is kind of a mix of Apache Camel, WebSockets, EventStream and HTTP(S).

# Reason
The idea behind this event system is to create multi client and server inter-messaging to create harmonious application platform where micro services can run independently.

To achieve this we wil be creating a generic process management, request forwarding and events service bus.

The system will pretty much work like below;
## Process manager
This will have only 1 endpoint where the processes register themselves, which is;
```
URL: POST api.plusclouds.com/v3/events/processes/register
params:
- name: The name of the application which should be registered to the events system. (Please look how to register for more info)
- endpoint: This will be the endpoint that the application accepts requests from.
- token: This will be the security token for us to recognize
```

**Sample request**
```
URL: POST api.plusclouds.com/v3/events/processes/register
params:
- name: iaas
- endpoint: http://10.128.0.100:8000,http://10.128.0.150:8000,http://10.128.0.200:8000
- token: Wld2pWduZW1hqgzbxSWDW5mhGzxZzD0I
```

#### Mechanism
The process manager will be responsible for scaling the application in the platform. This scaling is only limited with the modules that we manage on the same cluster. Like if we are managing the IaaS application and it requires scaling up, the process manager will make a request to the related platform to increase the number of docker instances running in the very same cluster.

## Events poller
This is where we use Nginx & Nchan or OpenResty & Nchan as a event poller and distributor. This part of the application is basicly built on nchan and it will only forward or distribute events.

#### Mechanism
This is pretty straight forward. We get the request through HTTPS, WebSocket or other channels, then forward it to the related channel.

The configuration file is saved in the nginx folder. Please take a look at that.

## Request Forwarder
This part will be responsible for keeping track of applications and application performance in terms of requests and responses. To be able to understand which application is taking too much requests and needs scaling.

#### Mechanism
We will be keeping track of past request, response data as well as time it takes to execute. From there we will be calculating average request per second and or maximum amount of request we can process in a second data. This will give us the idea to calculate each different requests cost. From that cost value we will be trying to understand the optimum cost per process and when the process starts to takes more time then we actually desire it will trigger the process manager to scale up the process.

Likewise this request forwarder will also calculate if we need to scale down the number or processes of this more than enough.

# Process Management
In the process management layer we will be keeping track of how many requests and/or background processes are being processed by the application. From this number we will try to decide how many workers we need for each process and from there we will orchestrate the application platform supported below. 

To keep track of this service, applications will first register themselves to this events service and only by then they will be able to send and/or receive certain events.


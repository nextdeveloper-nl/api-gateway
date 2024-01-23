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
- endpoint: This will be the endpoint that the application accepts requests from
- token: This will be the security token for us to recognize
```

## Events poller
This is where we use Nginx & Nchan or OpenResty & Nchan as a event poller and distributor. 

# Process Management
In the process management layer we will be keeping track of how many requests and/or background processes are being processed by the application. From this number we will try to decide how many workers we need for each process and from there we will orchestrate the application platform supported below. 

To keep track of this service, applications will first register themselves to this events service and only by then they will be able to send and/or receive certain events.


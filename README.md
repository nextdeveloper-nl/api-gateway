# NextDeveloper Events
Omni channel events service that will collect and redirect the related messages through various different channels and in between applications

# Reason
The idea behind this event system is to create multi client and server inter-messaging to create harmonious application platform where micro services can run independently.

To achieve this we wil be creating a generic process management, request forwarding and events service bus.

# Process Management
In the process management layer we will be keeping track of how many requests and/or background processes are being processed by the application. From this number we will try to decide how many workers we need for each process and we can orchestrate the required

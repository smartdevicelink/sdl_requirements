## Functional Requirements

1.
In case mobile application sends valid and allowed by Policies SubscribeVehicleData_request to SDL

SDL must: 
- transfer SubscribeVehicleData_request to HMI
- respond with `<resultCode>` received from HMI to mobile app 

2.
In case mobile application sends valid SubscribeVehicleData_request to SDL

and this request is NOT allowed by Policies

SDL must: 

respond "DISALLOWED, success:false" to mobile application

## Non-Functional Requirements

## Diagrams

SubscribeVehicleData

![]()

## Functional Requirements

1.
In case mobile application sent valid UnsubscribeVehicleData_request to SDL
- and this request is allowed by Policies
- and there are no other applications subscribed on vehicle info npotifications

SDL must: 
- transfer UnsubscribeVehicleData_request to HMI
- respond with `<resultCode>` received from HMI to mobile application

2.
In case mobile application sent valid UnsubscribeVehicleData_request to SDL
- and this request is allowed by Policies
- and there are other applications still subscribed on vehicle info npotifications

SDL must: 
- unsubscribe requesting application from waypoints change notifications
- not send UnsubscribeVehicleData_reques to HMI

## Non-Functiona Requirements

## Diagram

UnsubscribeVehicleData

![UnsubscribeVehicleData](.png)

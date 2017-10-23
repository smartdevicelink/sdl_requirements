## Functional Requirements

1.
In case iAP session was closed due to transport type change

SDL must:  
- keep all applications registered for reconnecting device  
- during `<AppTransportChangeTimer> + <AppTransportChangeTimerAddition>*N` reconnection timer

2.
In case due to transport change SDL started `<AppTransportChangeTimer> + <AppTransportChangeTimerAddition>*N` reconnection timer
and HMI sends any notifications and requests to `appID` that needs to be reconnected  

SDL must:  
- store all the notifications and requests from HMI internally  
- transfer all notifications and requests from HMI right after the `appID` will be reconnected

3.
In case due to transport change SDL started `<AppTransportChangeTimer> + <AppTransportChangeTimerAddition>*N` reconnection timer
and HMI sends any responses to `appID` on requests that were sent before reconnection  

SDL must:  
discard all responses to such `appID` from HMI

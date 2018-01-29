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

## Non-Functional Requirements
New parameters "AppTransportChangeTimer" , "AppTransportChangeTimerAddition"   must be added to smartdevicelink.ini file -> [MAIN] section

```
[MAIN] 
"AppTransportChangeTimer" = 500 ms 
Defines the timeout period SDL Core will wait for an application to re-register via alternate transport during 'transport switch'. If the application fails to re-register within this time, SDL Core shall inform HMI that the application has unregistered.

```

```
[MAIN] 
"AppTransportChangeTimerAddition" = 0ms
Defines the timeout  for waiting of every mobile app to reconnect between Bluetooth and USB transports change in case the number of connected apps is more than one
```

## Diagram
Reconnection timer and notifications from HMI 

![Reconnection timer and notifications from HMI](https://github.com/smartdevicelink/sdl_requirements/blob/feature/iap_over_bt/detailed_docs/accessories/iAP%20transport%20change%20reconnection.png)

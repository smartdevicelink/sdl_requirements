## Functional Requirements

1.
In case device was connected over Bluetooth and SDL received signal about USB connection of the same device

SDL must:

- start `<AppTransportChangeTimer> + <AppTransportChangeTimerAddition>*N` after sending switch signal to external system  
_Note:_ **N** is the number of applications that need to be re-registered

2.
In case mobile application sends RegisterAppInterface during `<AppTransportChangeTimer> + <AppTransportChangeTimerAddition>*N` timer
and `<UUID>` and `<appID>` parameters match with closed session

SDL must:
- consider such app as re-registered app
- update data in PolicyTable (`<device_identifier>` section)
- send UpdataDeviceList to HMI

_Note:_ For HMI during re-connection all applications remain registered and active, HMILevel resumption is not applicable

3.
In case mobile application was not re-registered during `<AppTransportChangeTimer> + <AppTransportChangeTimerAddition>*N`
due to transport type change

SDL must:

send BC.OnAppUnregistered (appID, unexpectedDisconnect:true) to HMI for such application

4.
In case mobile application sends RegisterAppInterface during `<AppTransportChangeTimer> + <AppTransportChangeTimerAddition>*N` timer  
and `<UUID>` of reconnected device matches  
and `<appID>` parameter does not match with closed session

SDL must:  
- register such app as a new app  
- send BC.OnAppRegistered to HMI  
- send BC.UpdateAppList to HMI

5.
In case mobile app sends RegisterAppInterface during `<AppTransportChangeTimer> + <AppTransportChangeTimerAddition>*N` timer  
and `<appID>` parameter matches with appID of closed session  
and `<UUID>` does not match  

SDL must:  
respond RegisterAppInterface (DUPLICATE_NAME, success:false)

6.
In case iOS device is connected over one of the available transports  

SDL must:  
write hashed `<UUID>` to `<device identifier>` in Device Data section of Policy Table

7.
In case iOS device was connected over USB  
and USB connection was lost  

SDL must:  
NOT start `<AppTransportChangeTimer> + <AppTransportChangeTimerAddition>*N` timer for reconnection  
_Note:_ In case of USB connection loss SDL must proceed with unexpected disconnect flow

## Non-Functional Requirements

```
[IAP] 
; defines the timeout for waiting the mobile app to reconnect between Bluetooth and USB transports change
"AppTransportChangeTimer" = 500 ms
```

```
[IAP] 
"AppTransportChangeTimerAddition" = 0ms
Defines the timeout  for waiting of every mobile app to reconnect between Bluetooth and USB transports change in case the number of connected apps is more than one
```


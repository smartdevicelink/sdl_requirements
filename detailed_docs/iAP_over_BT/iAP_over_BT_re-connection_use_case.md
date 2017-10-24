## Use Case 1: iAP over BT re-connection

**Main Flow:**  

_Pre-conditions:_  
a. iOS device is connected over Bluetooth  
b. App from iOS device is registered and running on SDL 

_Steps:_    
1. User connects the same iOS device connects over USB  

_Expected:_  

2. SDL receives signal about USB connection of the same device 
3. SDL sends signal about transport switch of the same device  
4. SDL receives `<UUID>` via IOSDeviceConnectInfo of the connected device 
5. SDL starts `<AppTransportChangeTimer> + <AppTransportChangeTimerAddition>*N` reconnection timer
6. SDL receives `<RegisterAppInterface>` request **before** timer expires
7. App sends **valid** "hashID" with `RegisterAppInterface` request 
8. SDL sends succesful response to the app (keep all data created/stored by this app BEFORE re-registration (transport switching)
9. SDL updates data in Policytable (`<device_identifier>` section) [write hashed `<UUID>` to `<device identifier>` in Device Data section of Policy Table]
10. SDL notifies HMI via `UpdatadeviceList` about changed `transportType`
11. SDL does not sent `BC.OnAppRegistered` to HMI


_Exception 1:_  
6.1. Timer expires with **no** `<RegisterAppInterface>` request from mobile application (either RegisterAppInterface was not sent or was sent but registration was rejected)  
6.1.a. SDL notifies HMI about app being unregistred via BC.OnAppUnregistered (appID, unexpectedDisconnect:true)  
6.1.b. App registers with **valid** "hashID"  
6.1.c. SDL performs data resumption for this app  
6.1.d. SDL **notifies** HMI about fresh app registration 

_Exception 2:_  
6.1.b.1. App registers with **invalid** or **missing** "hashID"   
6.1.b.1.a. SDL clears all resumption data related to this app  
6.1.b.1.b. SDL **notifies** HMI about fresh app registration (sends BC.OnAppRegistered to HMI send BC.UpdateAppList to HMI)

_Exception 3:_  
7.1. App sends **invalid** or omitted "hashID" with `RegisterAppInterface` request  
7.1.a. SDL responds with RESUME_FAILED to app's registration request  
7.1.b. SDL clears all resumption data related to this app **except** "AppIconsFolder"  
7.1.c. SDL does **not** notify HMI about new registration (HMI continues to display app screen)

**Alternative flow:**  

_Pre-conditions:_  
a. iOS device is connected over USB  
b. App from iOS device is registred and running on SDL  

_Steps:_    
1. USB transport was disconnected

_Expected:_   

2. SDL does not start `<AppTransportChangeTimer> + <AppTransportChangeTimerAddition>*N` reconnection timer  
3. SDL notifies HMI about app being unregistred

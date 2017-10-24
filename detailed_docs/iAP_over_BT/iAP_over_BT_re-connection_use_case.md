## Use Case 1: iAP over BT re-connection

**Main Flow:**  

_Pre-conditions:_  
a. iOS device is connected over Bluetooth  
b. App from iOS device is registred and running on SDL 

_Steps:_    
1. User connects the same iOS device connects over USB  

_Expected:_  

2. SDL receives signal about USB connection of the same device 
3. SDL sends signal about transport switch of the same device  
4. SDL receives `<UUID>` via IOSDeviceConnectInfo of the connected device 
5. SDL starts `<AppTransportChangeTimer> + <AppTransportChangeTimerAddition>*N` reconnection timer
6. SDL receives `<RegisterAppInterface>` request **before** timer expires
7. App sends **valid** "hashID" with `RegisterAppInterface` request 
8. SDL compares `<UUID>` and `<appID>` with previously closed  BT session
9. SDL sends succesful response to the app
10. SDL updates data in Policytable (`<device_identifier>` section)
11. SDL notifies HMI via `UpdatadeviceList` about changed `transportType`
12. SDL does not sent `BC.OnAppRegistered` to HMI

_Exception 1:_

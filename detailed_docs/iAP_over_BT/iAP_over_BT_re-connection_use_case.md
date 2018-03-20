## Use Case 1: iAP over BT re-connection

**Main Flow:**  

_Pre-conditions:_  
a. iOS device is connected over Bluetooth  
b. Application from iOS device is registered and running on SDL 

_Steps:_    
1. User connects the same iOS device over USB  

_Expected:_  

2. SDL receives signal with `<UUID>` about USB connection of the same device 
3. SDL notifies HMI about new connection via UpdateDeviceList (`<UUID>`)  
4. SDL responds ACK to connection signal
5. SDL receives signal from external system about transport switch of the same <UUID> device 
6. SDL substitutes iAP2BT adapter with iAP2USB adapter
7. SDL  responds ACK to switch signal
8. SDL starts reconnection timer
9. HMI disconnects the iAP channel over BT transport 
10. SDL notifies HMI via `UpdatadeviceList` about changed `transportType`
11. SDL receives `<RegisterAppInterface>` request before timer expires
12. Application sends valid "hashID" with `<RegisterAppInterface>` request
13. SDL updates data in PolicyTable 
14. SDL does not notify HMI about new registration


_Exception 1:_  
11.1. **After** timer expires application sends **valid** "hashID" with `<RegisterAppInterface>` request  
11.1.a. SDL notifies HMI about app being unregistered   
11.1.b. Application registers with **valid** "hashID"  
11.1.c. SDL performs data resumption for this application  
11.1.d. SDL **notifies** HMI about fresh application registration 

_Exception 2:_  
11.1.b.1. Application registers with **invalid** or **missing** "hashID"   
11.1.b.1.a. SDL clears all resumption data related to this application  
11.1.b.1.b. SDL **notifies** HMI about fresh application registration (sends BC.OnAppRegistered to HMI send BC.UpdateAppList to HMI)

_Exception 3:_  
12.1. Application sends **invalid** or omitted "hashID" with `RegisterAppInterface` request  
12.1.a. SDL responds with RESUME_FAILED to application's registration request  
12.1.b. SDL clears all resumption data related to this application **except** "AppIconsFolder"  
12.1.c. SDL does **not** notify HMI about new registration (HMI continues to display app screen)

## Use Case 2: USB connection loss  

_Pre-conditions:_  
a. iOS device is connected over USB  
b. Application from iOS device is registered and running on SDL  

_Steps:_    
1. USB transport was disconnected

_Expected:_   

2. SDL does not start reconnection timer  
3. SDL notifies HMI about application being unregistered

## Use Case 3: Multiple connection   
_Pre-conditions:_  
a. iOS device is connected over Bluetooth  
b. Application `<app_1>` from iOS device is registered and running on SDL  

_Steps:_
 1. User connects the same device over USB
 2. SDL receives signal with `<UUID>` via IOSDeviceConnectInfo
 3. SDL connects USB device
 4. HMI disconnects the iAP channel over BT transport
 5. SDL starts reconnection timer
 6. Application `<app_2>` sends RegisterAppInterface (params)  
 
 _Expected result:_  
 7. SDL compares `<UUID>` and `<appID>` parameters with previously closed session  
 8. SDL registers `<app_2>` as a new application  
 9. SDL sends BC.OnAppRegistered to HMI  
 
 _Exception 1:_   
 1.1. User connects another device over Bluetooth  
 1.1.a SDL receives signal with `<UUID_1>` about USB connection of the same device  
 1.1.b SDL receives signal with `<UUID_2>` about Bluetooth connection of another device  
 1.1.c SDL opens appropriate EAP protocols for `<UUID_1>` USB session  
 1.1.d SDL closes active EAP session opened for `<UUID_1>` Bluetooth  
 1.1.e SDL opens appropriate EAP protocols for `<UUID_2>` Bluetooth session  
 1.1.f SDL starts reconnection timer for `<UUID_1>`  
 1.1.g `<app_1>` from device `<UUID_2>` sends RegisterAppInterface (params)  
 
 _Expected result:_  
 1.1.h SDL compares `<UUID>` and `<appID>` parameters with previously closed session  
 1.1.i SDL responds to the app with RegisterAppInterface (DUPLICATE_NAME, success:false)

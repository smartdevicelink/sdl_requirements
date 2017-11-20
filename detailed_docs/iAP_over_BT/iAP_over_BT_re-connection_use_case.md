## Use Case 1: iAP over BT re-connection

**Main Flow:**  

_Pre-conditions:_  
a. iOS device is connected over Bluetooth  
b. App from iOS device is registered and running on SDL 

_Steps:_    
1. User connects the same iOS device over USB  

_Expected:_  

2. SDL receives signal with `<UUID>` about USB connection of the same device 
3. SDL compares `<UUID>` of BT device connection with `<UUID>` of USB connection  
4. `<UUID>` of BT device connection matches with `<UUID>` of USB connection
5. SDL sends signal about transport switch to the System  
6. SDL starts reconnection timer
7. SDL receives `<RegisterAppInterface>` request **before** reconnection timer expires
8. App sends **valid** "hashID" with `RegisterAppInterface` request 
9. SDL sends succesful response to the app 
10. SDL updates data in Policytable 
11. SDL notifies HMI via `UpdatadeviceList` about changed `transportType`
12. SDL does not notify HMI about new registration


_Exception 1:_  
7.1. **After** timer expires app sends **valid** "hashID" with `<RegisterAppInterface>` request  
7.1.a. SDL notifies HMI about app being unregistred   
7.1.b. App registers with **valid** "hashID"  
7.1.c. SDL performs data resumption for this app  
7.1.d. SDL **notifies** HMI about fresh app registration 

_Exception 2:_  
7.1.b.1. App registers with **invalid** or **missing** "hashID"   
7.1.b.1.a. SDL clears all resumption data related to this app  
7.1.b.1.b. SDL **notifies** HMI about fresh app registration (sends BC.OnAppRegistered to HMI send BC.UpdateAppList to HMI)

_Exception 3:_  
8.1. App sends **invalid** or omitted "hashID" with `RegisterAppInterface` request  
8.1.a. SDL responds with RESUME_FAILED to app's registration request  
8.1.b. SDL clears all resumption data related to this app **except** "AppIconsFolder"  
8.1.c. SDL does **not** notify HMI about new registration (HMI continues to display app screen)

**Use Case 2**  

_Pre-conditions:_  
a. iOS device is connected over USB  
b. App from iOS device is registred and running on SDL  

_Steps:_    
1. USB transport was disconnected

_Expected:_   

2. SDL does not start reconnection timer  
3. SDL notifies HMI about app being unregistred

**Use Case 3**  
_Pre-conditions:_  
a. iOS device is connected over Bluetooth  
b. App `<app_1>` from iOS device is registred and running on SDL  

_Steps:_
 1. User connects the same device over USB
 2. SDL receives signal with `<UUID>` via IOSDeviceConnectInfo
 3. SDL connects USB device
 4. SDL disconnects Bluetooth
 5. SDL starts reconnection timer
 6. App `<app_2>` sends RegisterAppInterface (params)  
 
 _Expected result:_  
 7. SDL compares `<UUID>` and `<appID>` parameters with previously closed session  
 8. SDL registers `<app_2>` as a new app  
 9. SDL sends BC.OnAppRegistered to HMI  
 
 _Exception 1:_   
 1.1. User connects another device over Bluetooth  
 1.1.a SDL receives signal with `<UUID_1>` about USB connection of the same device  
 1.1.b SDL receives signal with `<UUID_2>` about Bluetooth connection of another device  
 1.1.c SDL opens hub EAP protocol for `<UUID_1>` USB session  
 1.1.d SDL closes active EAP session opened for `<UUID_1>` Bluetooth  
 1.1.e SDL opens hub EAP protocol for `<UUID_2>` BT Bluetooth session  
 1.1.f SDL starts reconnection timer for `<UUID_1>`  
 1.1.g `<app_1>` from device `<UUID_2>` sends RegisterAppInterface (params)  
 Expected result:
 1.1.h SDL compares `<UUID>` and `<appID>` parameters with previously closed session  
 1.1.i SDL responds to the app with RegisterAppInterface (DUPLICATE_NAME, success:false)

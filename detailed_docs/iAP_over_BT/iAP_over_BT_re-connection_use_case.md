## Use Case 1: iAP over BT re-connection

**Main Flow:**  

_Pre-conditions:_  
a. iOS device is connected over Bluetooth  
b. App from iOS device is registered and running on SDL 

_Steps:_    
1. User connects the same iOS device over USB  

_Expected:_  

2. SDL receives signal with `<UUID>` about USB connection of the same device 
3. SDL sends signal about transport switch to the System  
4. SDL starts reconnection timer
5. SDL receives `<RegisterAppInterface>` request **before** reconnection timer expires
6. App sends **valid** "hashID" with `RegisterAppInterface` request 
7. SDL sends succesful response to the app 
8. SDL updates data in Policytable 
9. SDL notifies HMI via `UpdatadeviceList` about changed `transportType`
10. SDL does not notify HMI about new registration


_Exception 1:_  
6.1. **After** timer expires app sends **valid** "hashID" with `<RegisterAppInterface>` request  
6.1.a. SDL notifies HMI about app being unregistred   
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

**Use Case 2**  

_Pre-conditions:_  
a. iOS device is connected over USB  
b. App from iOS device is registred and running on SDL  

_Steps:_    
1. USB transport was disconnected

_Expected:_   

2. SDL does not start `<AppTransportChangeTimer> + <AppTransportChangeTimerAddition>*N` reconnection timer  
3. SDL notifies HMI about app being unregistred

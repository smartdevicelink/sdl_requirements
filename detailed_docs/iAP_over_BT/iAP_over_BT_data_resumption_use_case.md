## Use Case 1: iAP over BT - Data resumption

**Main Flow:**

_Pre-conditions:_  
a. iOS device is connected over Bluetooth  
b. App from iOS device is registred and running on SDL  

_Steps:_  
1. User connects the same iOS device over USB

_Expected:_  
2. SDL starts `<AppTransportChangeTimer> + <AppTransportChangeTimerAddition>*N` reconnection timer  
3. SDL receives `<RegisterAppInterface>` request **before** timer expires  
4. App sends **valid** "hashID" with `RegisterAppInterface` request  
5. SDL sends succesful response to the app  
6. SDL successfully resumes data for this app  
7. SDL does **not** notify HMI about new registration (HMI continues to display app screen)

_Exception 1:_  
3.1. Timer expires with **no** `<RegisterAppInterface>` request from mobile application  
3.1.a. SDL notifies HMI about app being unregistred  
3.1.b. App registers with **valid** "hashID"  
3.1.c. SDL performs data resumption for this app  
3.1.d. SDL **notifies** HMI about fresh app registration

_Exception 2:_  
3.1.b.1. App registers with **invalid** or **missing** "hashID"  
3.1.b.1.a. SDL clears all resumption data related to this app  
3.1.b.1.b. SDL **notifies** HMI about fresh app registration

_Exception 3:_  
4.1. App sends **invalid** "hashID" with `RegisterAppInterface` request  
4.1.a. SDL responds with RESUME_FAILED to app's registration request  
4.1.b. SDL clears all resumption data related to this app  
4.1.c. SDL does **not** notify HMI about new registration (HMI continues to display app screen)

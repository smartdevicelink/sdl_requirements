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
4. SDL **validates** "hashID" received with RegisterAppInterface request  
5. SDL sends succesful response to the app  
6. SDL successfully resumes data for this app

_Exception 1:_  
3.1.a SDL receives `<RegisterAppInterface>` request **after** timer expires  
3.1.b SDL **doesn't validate** "hashID" received with RegisterAppInterface request  
3.1.c SDL notifies HMI about app being unregistred  
3.1.d When the app re-registers SDL checks `<hashID>`  
3.1.e SDL  doesn’t  validates "hashID" received with `<RegisterAppInterface>` request  
3.1.f SDL clears all data related to the app

_Exception 2:_  
4.1.a SDL **doesn't validate** "hashID" received with `<RegisterAppInterface>`  request  
4.1.b SDL responds RESUME_FAILED, success:true to the app  
4.1.c SDL clears all data created/stored by this app  
4.1.d SDL sends to HMI following RPCs  
OnButtonSubscription (isSubscribed=false)  
UnsubscribeVehicleData  
UnsubscribeWayPoints
ResetGlobalProperties  
DeleteSubMenu  
DeleteCommand  
DeleteCommands for choiseSets (CreateInteractionChoiceSet)

**Alternative flow 1**  
3.a.1 SDL receives `<RegisterAppInterface>` request **after** timer expires  
3.a.2 SDL **validates** "hashID" received with RegisterAppInterface request  

_Expected:_  
1. SDL notifies HMI about app being unregistred 
2. SDL receives `<RegisterAppInterface>` request 
3. SDL validates "hashID" received with RegisterAppInterface request
4. SDL sends `<OnAppRegistered>` (resumeVRGrammars=true) to HMI
5. SDL sends succesful `<RegisterAppInterface>` response to the app
6. SDL performs data resumption

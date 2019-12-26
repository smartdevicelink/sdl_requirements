## UseCase 1: App data resumption after Ignition Off or Unexpected disconnect

**Main Flow:**

_Pre-conditions:_

a.	SDL and HMI are started  
b.	Mobile application is running on SDL  
c.	Mobile application has some data that can be resumed  
e. 	Disconnection on Ignition Off or Unexpected transport disconnect occurs

_Steps:_

1.	App sends RegisterAppInterface request (not later than 3 consecutive ignition cycles)

_Expected:_

2.	Policies rules and DUPLICATE_NAME check is performed
3.	App passed Policies check successfully, registration is valid
4.	SDL validates `hashID` provided with RegisterAppInterface request
5.	`hashID` equals stored value for the application
6.	SDL resumes app persistent data sending all required _RPCs from resumption list_
7.	SDL responds RegisterAppInterface(success: true, resultCode: SUCCESS) to the app  
8.  SDL notifies HMI by OnAppRegistered (resumeVrGrammars:true) to restore VRgrammars persisted on HMI


**Exception 1**  
3.1.a App registration is invalid due to Policies rules  
3.2.b SDL does not register the app and returns RegisterAppInterface(DUPLICATE_NAME, success: false) response  
3.2.c SDL does not clean up resumption data

**Exception 2**  
5.1.a `hashID` does not equal stored value  
5.1.b SDL interrupts data resumption process  
5.1.c SDL responds RegisterAppInterface (success: true, resultCode: RESUME_FAILED) to the app  
5.1.d SDL notifies HMI by OnAppRegistered (resumeVrGrammars:false) to restore VRgrammars persisted on HMI  
5.1.e SDL cleans up all stored application related data  
5.1.f SDL starts data persistence process from the beginning after app re-sends all application data to SDL 

**Alternative flow 1:** App doesn’t register during 3 consecutive ignition cycles  
1.a.1 App sends RegisterAppInterface request on the 4th ignition on  
1.a.2 SDL clears all corresponding application-related data used for resumption

## UseCase 2: Handle response from HMI during resumption data  
_Pre-conditions:_

a. SDL and HMI are started  
b. App with valid `hashID` reregisters after Ignition Off or Unexpected disconnect  

_Steps:_
1. App sends any RPC from resumption list  
2. HMI responds with any kind of error or does not respond

_Expected:_
3. SDL reverts already restored data with appropriate RPC  
4. After reverting persistent data SDL responds RegisterAppInterfaceResponse(success=true,result_code=RESUME_FAILED) to the app

**Exception 1**  
In the case some subscription data is already used by other applications, this means that the subscription is actual and 
SDL should not send unsubscribe requests to HMI.

## Use Case 3: Resumption of app's subscriptions
_Pre-conditions:_

a. SDL and HMI are started  
b. Mobile application is running on SDL  
c. Mobile application is subscribed to vehicle data (`param_1`)  
d. Mobile application (with valid `hashID`) reregisters after Ignition Off or Unexpected disconnect  

_Steps:_
1. App sends SubscribeVehicleData request   
2. SDL transfers the request to HMI during resumption
3. HMI responds with error resultCode  

_Expected:_ 
4. SDL processes unsuccess response from HMI
5. SDL responds RegisterAppInterfaceResponse(success=true,result_code=RESUME_FAILED) to the app

**Exception 1**  
c.1 Mobile application is subscribed to vehicle data (`param_1`, `param_2`)  
c.2 Mobile application (with valid `hashID`) reregisters after Ignition Off or Unexpected disconnect  
c.3 App sends SubscribeVehicleData request  
c.4 SDL transfers the request to HMI during resumption  
c.5 HMI responds with error resultCode for `param_1` and success resultCode for `param_2`  
_Expected:_  
c.6 SDL processes unsuccess response from HMI  
c.7 SDL removes subscription for `param_2` from HMI (by sending UnsubscribeVehicleData(`param_2`) request)  
c.8 SDL responds RegisterAppInterfaceResponse(success=true,result_code=RESUME_FAILED) to the app

## UseCase 4: Multiple apps subscription data resumption
_Pre-conditions:_

a. SDL and HMI are started  
b. App_1, App_2 are running on SDL    
c. App_1 is subscribed to vehicle data (`param_1`, `param_2`); App_2 is subscribed to (`param_2`, `param_3`)  
d. Ignition Off or Unexpected disconnect occurs

_Steps:_ 
1. App_1, App_2 reregister with `hashID`
2. App_1 sends request to restore subscriptions (`param_1`, `param_2`) during resumption
3. SDL transfers request to HMI  
4. HMI responds with error resultCode  
5. SDL responds RegisterAppInterfaceResponse(success=true,result_code=RESUME_FAILED) to App_1  
6. App_2 sends request to restore subscriptions (`param_2`, `param_3`)  

_Expected:_  
7. SDL transfers request to HMI  
8. HMI responds with success resultCode  
9. SDL restores vehicle data (`param_2`, `param_3`) for App_2  
10. SDL responds RegisterAppInterfaceResponse(success=true,result_code=SUCCESS) to App_2
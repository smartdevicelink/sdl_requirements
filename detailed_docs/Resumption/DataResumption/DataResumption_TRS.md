## Functional requirements

## Data Resumption

1. 
In case  
the app sends RegisterAppInterface_request with valid `hashID` due to disconnection on Ignition Off or Unexpected transport disconnect (not later than 3 consecutive Ignition cycles before)	
    
SDL must  
initiate app data resumption process

_Information: if app didn't register during 3 ign consecutive cycles -> SDL must delete app's related data from resumption list._

2.  

SDL must perform the check of `hashID` value of RegisterAppInterface request  
NOT earlier than DuplicateName and PolicyTable restrictions check during application registration  

3. 
In case of invalid app registation due to Policies rules  

SDL must  
NOT clean up resumption data

4.   
In case the application is connected from the same device and with the same Policy AppID number  

SDL must  
assign the same `internal_hmi_appID` for the application between ignition cycles

5. 
In case  
the app unregistered itself gracefully (e.g. user turn off mobile app via UnregisterAppInterface OR it's unregistered by SDL via OnAppUnregistered)  

SDL must  
NOT initiate app data resumption process

## Data Persistance

1. 
Every `AppSavePersistentDataTimeout` milliseconds defined in [smartDeviceLink.ini file](https://github.com/smartdevicelink/sdl_core/blob/master/src/appMain/smartDeviceLink.ini)  

SDL must  
persist in the database `<appID>`, `<hmi_appID>`, `<deviceID>`, `<isMedia>` (value requested by app and allowed by Policies)  
and successful application data updates 

Impacted RPCs:  
- AddCommand (Menu +VR) 
- AddSubMenu
- CreateInteractionChoiceSet
- SetGlobalProperties
- SubscribeButton 
- SubscribeVehicleData
- SubscribeWayPoints 
- DeleteCommand 
- DeleteSubMenu 
- DeleteInteractionChoiceSet 
- ResetGlobalProperties 
- UnsubscribeButton 
- UnsubscribeVehicleData 
- UnsubscribeWayPoints

2. 
In case  
SDL receives OnExitAllApplications(SUSPEND) from HMI  

SDL must 
1. Store all applications resumption-related data on the file system
2. Strore correct `<isMedia>` value for each app received in RegisterAppInterface_request and allowed by Policies to resumption database  
3. Continue generating resumption related data (in case getting some from mobile application) without sending OnHashChanged notification to mobile app

_Information: HMI closes both BT and USB transports together with sending BC.OnExitAllApplications (SUSPEND). SDL is not in time to unregister all of the applications. SDL will resume them upon the next ign_on._

3.   
In case  
SDL receives OnExitAllApplications(IGNITION_OFF)  

SDL must  
1. clean up any resumption-related data obtained after OnExitAllApplications(SUSPEND)  
2. stop all its processes, notify HMI via OnSDLClose and shut down

_Note: OnAppInterfaceUnregistered(Ignition_OFF) must not be sent to mobile applications on IGN_OFF

4.  
In case  
SDL receives OnAwakeSDL notification from HMI after the preceding OnExitAllApplications(SUSPEND)  

SDL must 
1. store an application resumption-related data it received from the application during SUSPEND state into DB  
2. send a single OnHashChanged notification with new `hashID` to mobile app


5.SDL must wait for the reconnection and store the app-related data for resumption during three consequentive ignition cycles only 
and clear all app-persisted data on the 4th IGNITION_ON

6. 
In case   
the app failed to be registered by DUPLICATE_NAME reason after unexpected_disconnect/IGN_OFF  

SDL must  
NOT clean up applicaton-persistant data  

## HashID and resume concept  

1. 
SDL must resume persistent data only 
if receives the same value of `hashID` (by RegisterAppInterface request) as stored before for this applicaton

2.  
SDL must not resume application data 
if receives NO `hashID`  (by RegisterAppInterface request) or the value different from `hashID` stored on SDL for the appropriate application 
In case SDL has previously stored data for the mentioned application, the app-persisted data must be cleaned up by SDL

3. 
SDL must update `hashID` value and send OnHashChange notification to mobile app  
after each of the following RPCs being executed with "success:true" and responded to mobile application: 

- AddCommand (Menu or/and VR)
- AddSubMenu
- CreateInteractionChoiceSet
- SetGlobalProperties
- SubscribeButton 
- SubscribeVehicleData
- SubscribeWayPoints
- DeleteCommand 
- DeleteSubMenu 
- DeleteInteractionChoiceSet 
- ResetGlobalProperties 
- UnsubscribeButton 
- UnsubscribeVehicleData
- UnsubscribeWayPoints

Information:
"success:true":  
-> WARNINGS  
-> SUCCESS  
-> UNSUPPORTED_RESOURCE (only in case `<Interface>` is supported by system -> HMI respond with <Interface>.IsReady (available:true) to SDL)  
-> RETRY  
-> SAVED  
-> WRONG_LANGUAGE

4.  
In case  
`hashID` of the application registered equals stored value for the application 

SDL must  
register the application according to standard process: 

a. return RegisterAppInterface response of (success: true, resultCode: SUCCESS, info "Resume succeeded") after successfull registering  
b. notify HMI by OnAppRegistered (resumeVrGrammars:true) to restore VRgrammars persisted on HMI  
c. Restore application related data: 
- AddCommand (Menu +VR)
- AddSubMenu
- CreateInteractionChoiceSet
- SetGlobalProperties
- SubscribeButton
- SubscribeVehicleData
- SubscribeWayPoints

d. Send the following restored data to HMI right after OnAppRegistered notification sent to HMI:
- AddCommand (Menu only)
- AddSubMenu
- CreateInteractionChoiceSet (AddCommands of the ChoiceSets)
- SetGlobalProperties
- OnButtonSubscription (isSubscribed=true) 
- SubscribeVehicleData
- SubscribeWayPoints

5. 
In case  
`hashID` of the application registered doesn't equal stored value for the application 

SDL must  
interrupt data resumption process: 

a. return RegisterAppInterface_response (success: true, resultCode: RESUME_FAILED)  
b. notify HMI by OnAppRegistered (resumeVrGrammars:false) to restore VRgrammars persisted on HMI  
c. clean up all stored application related data: 
- AddCommand (Menu +VR)
- AddSubMenu
- CreateInteractionChoiceSet (AddCommands of the ChoiceSets)
- SetGlobalProperties
- SubscribeButton
- SubscribeVehicleData
- SubscribeWayPoints

d. start data persistance process from the begining for the current application

_Note: It's application's responsibility to re-send all application data to SDL after RESUME_FAILED_  
_Note: HMI cleans up persisted VrGrammars data by itself after getting resumeVrGrammars:false_

6.  
In case  
HMI responds with any kind of error or does not respond to any of requests

SDL must  
revert already restored data with appropriate RPC  
respond RegisterAppInterfaceResponse(success=true,result_code=RESUME_FAILED) to the app

## Non-functional requirements

1. `AppSavePersistentDataTimeout` value defined in smartDeviceLink.ini file - Timeout in milliseconds for periodical saving resumption persistent data

2.  SDL must store resumption-related data in a separate Resumption database.

3. The "UseDBForResumption" parameter defines the possible way to store resumption data:
-> in case the value "true" = should be stored to database
-> in case the value "false" = should be stored as JSON file

_Information: in case UseDBForResumption is omitted or empty, SDL uses app_info.dat for storing resumption-related data._

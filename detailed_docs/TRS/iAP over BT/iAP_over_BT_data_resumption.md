## Functional Requirements

1. 
In case application re-registers during `<AppTransportChangeTimer> + <AppTransportChangeTimerAddition>*N`  
and sends valid `hashID` param via RegisterAppInterface_request to SDL  

SDL must:  
- respond RegisterAppInterface (SUCCESS, success:true) to mobile app (in case of no other failures)  
- keep all data created/stored by this app BEFORE re-registration (transport switching)

2.  
In case application re-registers during `<AppTransportChangeTimer> + <AppTransportChangeTimerAddition>*N`  
and sends invalid `hashID` param via RegisterAppInterface_request to SDL  
OR omits `hashID` at all  

SDL must:  
- respond RegisterAppInterface (RESUME_FAILED, success:true) to mobile app (in case of no other failures)  
- clear all data related to this app internally except "AppIconsFolder"  
- send requests to remove data in HMI

3.  
In case application re-register AFTER `<AppTransportChangeTimer> + <AppTransportChangeTimerAddition>*N` expiration  
and sends valid `hashID` param via RegisterAppInterface_request to SDL   

SDL must:  
- notify HMI about app being unregistered
- start data resumption process according to existing rules when app re-registers if `hashID` is valid

4. 
In case application re-registers AFTER `<AppTransportChangeTimer> + <AppTransportChangeTimerAddition>*N` expiration 
and sends invalid `hashID` param via RegisterAppInterface_request to SDL  
or omits `hashID` at all  

SDL must:  
behave according to existing rules 


## Use Case 1: Handling notifications from HMI during re-connection

**Main Flow:**

_Pre-conditions:_  
a. iOS device is connected over Bluetooth  
b. Device is consented and N applications from this device are registered in SDL  
 
_Steps:_  
 1. User connects the same device over USB
 
 
_Expected:_   
 2. SDL starts `<AppTransportChangeTimer> + <AppTransportChangeTimerAddition>*N` timer for reconnection  
 3. SDL does not unregister the applications until `<AppTransportChangeTimer> + <AppTransportChangeTimerAddition>*N` timer expires

**Alternative flow 1**  
3.a.1 HMI sends any notifications and requests to <appID> that needs to be reconnected  

_Expected:_  

3.a.2 SDL stores all the notifications and requests from HMI internally  
3.a.3 SDL transfers all notifications and requests from HMI right after the `<appID>` will be reconnected 
 
**Alternative flow 2**  
3.b.1 HMI sends any responses to `<appID>` on requests that were sent before reconnection  

_Expected:_    
3.b.2 discard all responses to such `<appID>` from HMI

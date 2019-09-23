### Module Allocation rules

To allocate a module mobile app sends one of the following RPCs
* SetInteriorVehicleData 
* ButtonPress

1.
In case 

RC app sends RPC(moduleType + moduleId) request to allocate a free module  
and `userLocation` is within `moduleInfo.serviceArea`

SDL must  
allocate the module and send a successfull response 

2.

In case 

RC app sends RPC(moduleType + moduleId) request to allocate a free module  
and `userLocation` is NOT within `moduleInfo.serviceArea`

SDL must  
reject allocation module request

3.
In case 

RC app sends RPC(moduleType + moduleId) request to allocate a module already allocated by another app  
and `moduleInfo.allowMultipleAccess=false`

SDL must  
reject allocation module request

4.
In case 

RC app sends RPC(moduleType + moduleId) request to allocate a module already allocated by another app  
and `moduleInfo.allowMultipleAccess=true`  
and `userLocation` is NOT within `moduleInfo.serviceArea`

SDL must  
reject allocation module request

5.
In case 

RC app sends RPC(moduleType + moduleId) request to allocate a module already allocated by another app  
and `moduleInfo.allowMultipleAccess=true`  
and `userLocation` is within `moduleInfo.serviceArea`  
and RCAccessMode = AUTO_ALLOW

SDL must  
allocates the module(`moduleType` + `moduleId`) without asking the driver

6.
In case 

RC app sends RPC(`moduleType` + `moduleId`) request to allocate a module already allocated by another app  
and `moduleInfo.allowMultipleAccess=false`  
and `userLocation` is within `moduleInfo.serviceArea`  
and RCAccessMode = AUTO_DENY

SDL must
reject allocation module request with resultCode:IN_USE

7.
In case 

RC app sends RPC(moduleType + moduleId) request to allocate a module already allocated by another app  
and `moduleInfo.allowMultipleAccess=true`  
and `userLocation` is within `moduleInfo.serviceArea`  
and RCAccessMode = ASK_DRIVER

SDL must  
1. check if the consent is cached  
1.a in case the consent is cached -> SDL returnes cached result  
1.b in case the consent is NOT cached -> SDL asks driver’s permission sending [GetInteriorVehicleDataConsent](https://github.com/smartdevicelink/sdl_requirements/blob/develop/detailed_docs/TRS/RC/GetInteriorVehicleDataConsent.md) to HMI

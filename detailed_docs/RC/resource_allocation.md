## Use Case 1: Resource allocation 

**Main Flow:**

_Pre-conditions:_

a. HMI and SDL are started, RC interface is available on HMI

b. RC_app_1 and RC_app_2 are registered on SDL with "REMOTE_CONTROL" appHMIType

c. module_1 is in "free" state on HMI

_Steps:_

1. User chose to allow RC functionality via HMI and specified access mode as AUTO_ALLOW or didn't specify the access mode at all
2. HMI sends notification to SDL that Remote control functionality is allowed with or without specified access mode
3. RC_app_1 is in HMILevel FULL sends request to allocate the module_1 (either  to change settings or emulate button press)
4. RC_app_1 is in control of the the module_1
5.	SDL sends OnRCStatus to HMI (app_1, allocatedModules:module_1; freeModules:module_2, module_3); (app_2, allocatedModules:(); freeModules:module_2, module_3)
6.	SDL sends OnRCStatus to RC app_1 (allocatedModules:module_1; freeModules:module_2, module_3)
7.	SDL sends OnRCStatus to RC app_2 (allocatedModules:(); freeModules:module_2, module_3)
8.	User activates RC_app_2 (app_2 is in FULL HMI level now)
9.	RC_app_2 sends control request to module_1 (sent request to either change settings or emulate button press)

_Expected:_

8. RC_app_2 gets in control of the module_1  
9. SDL sends OnRCStatus to HMI (app_1, allocatedModules:(), freeModules:module_2, module_3); (app_2, allocatedModules:module_1; freeModules:module_2, module_3)  
10. SDL sends OnRCStatus to RC app_1 (allocatedModules:(); freeModules:module_2, module_3)  
11. SDL sends OnRCStatus to RC app_2 (allocatedModules:module_1; freeModules:module_2, module_3)

_Exception 1:_

8.1 User did not activate RC_app_2 (app_2 is in LIMITTED or BACKGROUND HMILevel)

8.2 RC_app_2 sends control request to module_1 (sent request to either change settings or emulate button press)

8.3 RC_app_2 control request was rejected with result code REJECTED, success:false, RC_app_1 remains in control of module_1   

8.4 SDL does not send OnRCStatus to HMI and to registered application 

_Exception 2:_

4.1 RC_app_1  is in control of the module_1 (sent request to either change settings or emulate button press)

4.2 User activates RC_app_2 (app_2 is in FULL HMI level now)

4.3 module_1 is currently executing one of the RC requests of RC_app_1

4.4 RC_app_2 sends control request to module_1 (sent request to either change settings or emulate button press)

4.5 RC_app_2 control request was rejected with result code IN_USE, RC_app_1 remains in control of module_1  

4.6 SDL does not send OnRCStatus to HMI and to registered application

**Alternative flow 1**

1.a.1 User chose allow RC functionality via HMI and specified access mode as AUTO_DENY

1.a.2 HMI sends notification to SDL that Remote control functionality is allowed with specified access mode

1.a.3 RC_app_1 is in HMILevel FULL and is in control of the module_1 (sent request to either change settings or emulate button press)

1.a.4 User activates RC_app_2 (app_2 is in FULL HMI level now)

1.a.5 RC_app_2 sends control request to module_1 (sent request to either change settings or emulate button press)

_Expected:_

1.a.6 RC_app_2 control request was rejected with result code IN_USE, RC_app_1 remains in control of module_1  

1.a.7. SDL does not send OnRCStatus to HMI and to registered application

_Exception 1.1:_

1.a.4.a User activates RC_app_2 (app_2 is in FULL HMI level now) and closed RC_app_1 (RC_app_1 unregistered from SDL)  

1.a.4.b RC_app_1  successfuly unregistered.  

1.a.4.c SDL sends OnRCStatus to HMI and to RC_app_2 with all modules in `freeModules`  

1.a.4.e RC_app_2 sends control request to module_1 (sent request to either change settings or emulate button press)  

1.a.4.f RC_app_2 gets in control of the module_1   

1.a.4.g SDL sends OnRCStatus to HMI and to app2

_Exception 1.2:_  

1.a.4.b.1 RC_app_1  sends invalid json in unregister request  

1.a.4.b.2 Unregistration is failed  

1.a.4.b.3 SDL does not send OnRCStatus to HMI and to registered applications

**Alternative flow 2**

1.b.1 User chose allow RC functionality via HMI and specified access mode as ASK_DRIVER

1.b.2 HMI sends notification to SDL that Remote control functionality is allowed with specified access mode

1.b.3 RC_app_1 is in HMILevel FULL and is in control of the module_1 (sent request to either change settings or emulate button press)

1.b.4 User activates RC_app_2 (app_2 is in FULL HMI level now)

1.b.5 RC_app_2 sends control request to module_1 (sent request to either change settings or emulate button press)

_Expected:_

1.b.6 SDL sends request to HMI to display permission prompt to the driver

1.b.7 Pop-up on the screen of head unit asks driver to allow RC_app_2 access to module_1 

1.b.8 Driver allows access for RC_app_2 to get in control of module_1

1.b.9 HMI responds to SDL with user consent (allowed: true) regarding  RC_app_2 access to module_1

1.b.10 RC_app_2 gets in control of the module_1

_Exception 2.1:_

1.b.9.1 HMI does not respond during default time out or responds with invalid parameters

1.b.9.2 SDL responds with result code GENERIC_ERROR to RC_app_2 control request, RC_app_1 remains in control of module_1.  

1.b.9.3 SDL does not send OnRCStatus to HMI and to registered applications 
New control requests from RC_app_2 to module_1 if it is still in use by another application shall trigger new driver consent requests.  


_Exception 2.2:_

1.b.4.a User activates RC_app_2 (app_2 is in FULL HMI level now) and closed RC_app_1 (RC_app_1 unregistered from SDL)

1.b.4.b RC_app_1 successfuly unregistered  

1.b.4.c SDL sends OnRCStatus to HMI and to RC_app_2 with all modules in `freeModules`  

1.b.4.e RC_app_2 sends control request to module_1 (sent request to either change settings or emulate button press)  

1.b.4.f RC_app_2 gets in control of the module_1  

1.b.4.g SDL sends OnRCStatus to HMI and to RC_app_2

**Alternative flow 2.1**

1.b.8.a.1 Driver denies access for RC_app_2 to get in control of module_1

_Expected:_

1.b.8.a.2  HMI responds to SDL with user consent (allowed:false) regarding RC_app_2 access to module_1

1.b.8.a.3 RC_app_2 control request is rejected with result code REJECTED, success:false,  info: "The resource is in use and the driver disallows this remote control RPC", RC_app_1 remains in control of module_1

1.b.8.a.4 RC_app_2 requests control again for the same module_1

1.b.8.a.5 SDL rejects the request without new driver's permission request with result code REJECTED, success:false  

1.b.8.a.6 SDL does not send OnRCStatus to HMI and to registered applications

**Alternative flow 2.2**

1.b.8.b.1 Driver did not respond on consent prompt during default timeout

_Expected:_

1.b.8.b.2 HMI responds to SDL with result code TIMED_OUT

1.b.8.b.3 RC_app_2 control request is rejected with result code TIMED_OUT, success: false

1.b.8.b.4 SDL does not send OnRCStatus  

info: "The resource is in use and the driver did not respond in time", RC_app_1 remains in control of module_1

1.b.8.b.5 RC_app_2 requests control again for the same module_1  

1.b.8.b.6 SDL sends new request to HMI to display permission prompt to the driver

## Use Case 2: Resource allocation after RC disabling/enabling

_Pre-conditions:_

a. HMI and SDL are started, RC interface is available on HMI

b. RC_app_1 and RC_app_2 are registered on SDL with "REMOTE_CONTROL" appHMIType

c. RC functionality is enabled and "accessMode" is "AUTO_DENY"

d. module_1 is in use by RC_app_1

_Steps:_

1. User disables RC functionality
2. HMI sent notification to SDL that RC functionality is disabled
3. SDL assigns HMILevel NONE to RC_app_1 and RC_app_2 and unsubscribes applications from module_1  
4. SDL sends OnRCStatus notification to RC_app_1 and RC_app_2 and to HMI with module_1 in `freeModules` parameter
4. User activates RC functionality with "accessMode" = "ASK_DRIVER"
5. User activates RC_app_2, module_1 gets in "free" state again
6. RC_app_2 requests the control of module_1

_Expected:_

7. RC_app_2 gets in control of module_1 without asking a driver  
8. SDL sends OnRCStatus HMI (app_1, allocatedModules:(), freeModules:module_2, module_3); (app_2, allocatedModules:module_1; freeModules:module_2, module_3)  
9. SDL sends OnRCStatus to RC_app_1 (allocatedModules:(); freeModules:module_2, module_3)  
10. SDL sends OnRCStatus to RC_app_2 (allocatedModules:module_1; freeModules:module_2, module_3) and to 
11. User activates RC_app_1 
12. RC_app_1 requests the control of module_2  
13. SDL sends the request to HMI to display permission prompt to the driver
14. Driver allows, RC_app_1 gets control of module_2  
15. SDL sends OnRCStatus notification to HMI (app_1, allocatedModules:module_2, freeModules:module_3); (app_2, allocatedModules:module_1; freeModules:module_3)     
16. RC_app_1 (allocatedModules:module_2; freeModules:module_3)  
17. RC_app_2 module_2 (allocatedModules:module_1; freeModules:module_3)

_Exception:_

5.1 User activates RC functionality without choosing "accessMode"

5.2 SDL is using "accessMode" stored before disabling RC functionality ("AUTO_DENY")

5.3 User activates RC_app_2, module_1 is in "free" state again

5.4 RC_app_2 requests the control of module_1

5.5 RC_app_2 gets in control of module_1 without asking a driver

5.6 User activates RC_app_1 and RC_app_1 requests the control of module_1

5.7 RC_app_1 control request was rejected without asking a driver with result code IN_USE, RC_app_2 remains in control of module_1

## Use Case 3: Resource allocation cases to release the RC module

**Main Flow:**

_Pre-conditions:_

a. HMI and SDL are started, RC interface is available on HMI

b. SDL is built with "-DEXTENDED_POLICY: PROPRIETARY" flag or without this flag at all

c. RC_app_1 and RC_app_2 are registered on SDL with "REMOTE_CONTROL" appHMIType

d. module_1 is assigned in app's specific policies for RC_app_1 and RC_app_2

e. module_1 is in "in use" state on HMI and allocated to RC_app_1

_Steps:_

1. SDL unregisters the RC_app_1 by any reason (mobile appication sent UnregisterAppInterface, unexpected disconnect, master/factory defaults reset, protocol violation etc.)

_Expected:_

2. SDL releases module_1 from RC_app_1 control and unsubscribes RC_app_1 from receiving notifications about module_1 settings changed
3. SDL sends OnRCStatus to HMI and to RC_app2 with module_1 in `freeModules`  
4. RC_app_2 requests the control of module_1
5. RC_app_2 gets in control of module_1

**Alternative Flow 1:**

1.a.1 SDL received OnExitApplication (either user exits RC_app_1 vis HMI (either by UI or VR) or due to driver distraction violation)

1.a.2 RC_app_2 requests the control of module_1

_Expected:_

1.a.3 SDL assigns HMILevel NONE to RC_app_1, releases module_1 from RC_app_1 control and unsubscribes RC_app_1 from receiving notifications about module_1 settings changed  

1.a.4 SDL sends OnRCStatus to HMI, RC_app_1 and RC_app_2 with module_1 in `freeModules` parameter    

1.a.5 RC_app_2 gets in control of module_1

**Alternative Flow 2:**

1.b.1 Any trigger of Policy Table Update happened and in received PTU module_1 is not in the list of assigned moduleTypes for RC_app_1

_Expected:_

1.b.3 SDL disallows all RPC for module_1 from RC_app_1, releases module_1 from RC_app_1 control and unsubscribes RC_app_1 from receiving notifications about module_1 settings changed

1.b.4 SDL send OnRCStatus to HMI, RC_app_1 and RC_app_2 with module_1 in `freeModules` parameter

_Excpetion 2.1_

1.b.1.1 Any trigger of Policy Table Update happened and in received PTU RC_app_1 is revoked

1.b.1.2 SDL releases module_1 from RC_app_1 control  

1.b.1.3 SDL sends OnRCStatus to HMI, RC_app_1 and RC_app_2 with module_1 in `freeModules` parameter

**Alternative Flow 3:**

_Pre-conditions:_

a.3 HMI and SDL are started, RC interface is available on HMI

b.3 SDL is built with "-DEXTENDED_POLICY: EXTERNAL_PROPRIETARY" flag 

c.3 RC_app_1 and RC_app_2 are registered on SDL with "REMOTE_CONTROL" appHMIType

d.3 module_1 is assigned in app's specific policies for RC_app_1 and RC_app_2

e.3 module_1 is in "in use" state on HMI and allocated to RC_app_1

f.3 RC_app_1 and RC_app_2 are running on deviceID

_Steps:_

1.c.1 SDL received OnAllowSDLFunctionality (deviceInfo (deviceID), allowed: FALSE)

1.c.2 RC_app_2 requests the control of module_1

_Expected:_

1.c.3 SDL assigns HMILevel NONE to all applications registered from deviceID signed in deviceInfo and unsubscribes all applications of this device from all modules

1.c.4 module_1 is not allocated to any application  

1.c.5  SDL sends OnRCStatus to HMI, RC_app_1 and RC_app_2 with module_1 in `freeModules` parameter

_Exception 3.1_

1.c.1.1 SDL received OnAllowSDLFunctionality (allowed: FALSE) without specified deviceInfo

1.c.1.2 SDL assigns HMILevel NONE to all applications registered from all devices and unsubscribes all applications from all modules

1.c.1.3 module_1 is not allocated to any application  

1.c.1.4 SDL sends OnRCStatus to HMI, RC_app_1 and RC_app_2 with module_1 in `freeModules` parameter

> Requirement: [#10](https://github.com/smartdevicelink/sdl_requirements/issues/10) [SDL_RC] Resource allocation based on access mode

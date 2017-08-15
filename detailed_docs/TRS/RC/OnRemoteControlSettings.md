## Functional Requirements

1. 
In case
- SDL received OnRemoteControlSettings notification from HMI with allowed:true
- and "accessMode" = "AUTO_ALLOW" 
- and RC_module on HMI is alreay in control by RC-application

SDL must

- provide access to RC_module for the second RC_application in HMILevel FULL after it sends control RPC (either SetInteriorVehicleData or ButtonPress) for the same RC_module without asking a driver
- process the request from the second RC_application

*Information:*

a. in case HMI didn't provide "accessMode" parameter during start up or when RC feature was enabled SDL must set "accessMode" = "AUTO_ALLOW" by default (see p. 8)

b. in case HMI didn't provide "accessMode" parameter in current OnRemoteControlSettings notification but this parameter was received in previous OnRemoteControlSettings notification, SDL must use previously received "accessMode"

2.
In case
- SDL received OnRemoteControlSettings notification from HMI with allowed:true
- and with any "accessMode" or without "accessMode" parameter at all
- and RC_module on HMI is alreay in control by RC-application_1
- and another RC_application_2 is in HMILevel other than FULL (either LIMITED or BACKGROUND)

SDL must
- deny access to RC_module for another RC_application_2 after it sends control RPC (either SetInteriorVehicleData or ButtonPress) for the same RC_module without asking a driver
- not process the request from RC_application_2 and respond with result code REJECTED, success:false
- leave RC_application_1 in control of the RC_module

3.
In case
- SDL received OnRemoteControlSettings notification from HMI with allowed:true
- and "accessMode" = "AUTO_ALLOW" 
- and RC_module on HMI is alreay in control by RC-application_1
- and RC_module is currently executing request by RC_application_1
- and another RC_application_2 in HMILevel FULL sends control RPC (either SetInteriorVehicleData or ButtonPress)

SDL must
- deny access to RC_module for RC_application_2 without asking a driver
- not process the request from RC_application_2 and respond with result code IN_USE, success:false
- leave RC_application_1 in control of the RC_module

*Information:*

a. in case HMI didn't provide "accessMode" parameter during start up or when RC feature was enabled SDL must set "accessMode" = "AUTO_ALLOW" by default (see p. 8)

b. in case HMI didn't provide "accessMode" parameter in current OnRemoteControlSettings notification but this parameter was received in previous OnRemoteControlSettings notification, SDL must use previously received "accessMode"

4. 
In case
- SDL received OnRemoteControlSettings notification from HMI with allowed:true
- and parameter "accessMode" = "AUTO_DENY" 
- and RC_module on HMI is alreay in control by RC-application

SDL must
- deny access to RC_module for another RC_application in HMILevel FULL after it sends control RPC (either SetInteriorVehicleData or ButtonPress) for the same RC_module without asking a driver
- respond with result code IN_USE, success:false

5. 
In case
- SDL received OnRemoteControlSettings notification from HMI with allowed:true
- and parameter "accessMode" = "ASK_DRIVER" 
- and RC_module on HMI is alreay in control by RC-application_1

SDL must
- send GetInteriorVehicleDataConsent to HMI to display permission prompt to the driver after another RC_application_2 in HMILevel FULL sends control RPC (either SetInteriorVehicleData or ButtonPress) to the same RC_module 

6. 
In case

SDL received OnRemoteControlSettings (allowed:false) from HMI

SDL must
- store RC state allowed:false internally
- assign HMILevel none to all registered applications with appHMIType REMOTE_CONTROL and send OnHMIStatus (NONE) to such apps
- keep all applications with appHMIType REMOTE_CONTROL registered 
- unsubscribe all REMOTE_CONTROL applications from OnInteriorVehicleData notifications for all HMI modules internally

Note: HMILevel NONE cannot be changed for all REMOTE_CONTROL applications when RC-functionality is disabled

7.
In case
- RC_functionality is disabled on HMI
- and HMI sends notification OnRemoteControlSettings (allowed:true, "any_accessMode")

SDL must
- store RC state allowed:true and "accessMode" received from HMI internally
- allow RC functionality for applications with REMOTE_CONTROL appHMIType

Note: When RC functionality is enabled HMIlevel of REMOTE_CONTROL applications can be changed to other than NONE

8.
In case

HMI didn't send OnRemoteControlSettings notifications on systems start

SDL must

use default value allowed:true and accessMode = "AUTO_ALLOW" for all registered REMOTE_CONTROL applications until OnRemoteControlSettings notification with other settings is received

9.
In case
- on system start up HMI sent OnRemoteControlSettings notification with "accessMode" pasrameter
- and **without** "allowed" parameter

SDL must
- use default value allowed: true and received "accessMode" until for all registered REMOTE_CONTROL applications until OnRemoteControlSettings notification with other settings is received
- allow RC functionality for applications with REMOTE_CONTROL appHMIType


## Diagrams

OnRemoteControlSettings for Resource Allocation
![OnRemoteControlSettings](https://github.com/smartdevicelink/sdl_requirements/blob/OnRemoteControlSettings/detailed_docs/accessories/OnRemoteControlSettings.png)

OnRemoteControlSettings disabling/enabling RC functionality
![OnRemoteControlSettings disabling/enabling RC functionality](https://github.com/smartdevicelink/sdl_requirements/blob/OnRemoteControlSettings/detailed_docs/accessories/OnRemoteControlSettings_disablingRC.png)

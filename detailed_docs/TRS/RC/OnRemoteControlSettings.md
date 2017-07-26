## Functional Requirements

1. 
In case
- SDL received OnRemoteControlSettings notification from HMI with allowed:true
- and "accessMode" = "AUTO_ALLOW" or without "accessMode" parameter at all
- and RC_module on HMI is alreay in control by RC-application

SDL must

- provide access to RC_module for the second RC_application in HMILevel FULL after it sends control RPC (either SetInteriorVehicleData or ButtonPress) for the same RC_module without asking a driver
- process the request from the second RC_application

2.
In case
- SDL received OnRemoteControlSettings notification from HMI with allowed:true
- and with any "accessMode" or without "accessMode" parameter at all
- and RC_module on HMI is alreay in control by RC-application_1
- and another RC_application_2 is in HMILevel other than FULL (either LIMITTED or BACKGROUND)

SDL must
- deny access to RC_module for another RC_application_2 after it sends control RPC (either SetInteriorVehicleData or ButtonPress) for the same RC_module without asking a driver
- not process the request from RC_application_2 and respond with result code IN_USE, success:false
- leave RC_application_1 in control of the RC_module

3.
In case
- SDL received OnRemoteControlSettings notification from HMI with allowed:true
- and "accessMode" = "AUTO_ALLOW" or without "accessMode" parameter at all
- and RC_module on HMI is alreay in control by RC-application_1
- and RC_module is currently executing request by RC_application_1
- and another RC_application_2 in HMILevel FULL sends control RPC (either SetInteriorVehicleData or ButtonPress)

SDL must
- deny access to RC_module for RC_application_2 without asking a driver
- not process the request from RC_application_2 and respond with result code IN_USE, success:false
- leave RC_application_1 in control of the RC_module

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
- apply access to RC_module for RC_application_2 based on driver's decision  

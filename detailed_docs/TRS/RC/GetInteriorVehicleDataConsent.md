## Functional Requirements

### AUTO_ALLOW

1.	
In case
`accessMode` is set to `AUTO_ALLOW`  
and SDL receives GetInteriorVehicleDataConsent request from mobile app with 

- valid `moduleIds` with `userLocation` within `serviceArea` of each requested module or it is driver location
- (allowMultipleAccess = true)

SDL must
- respond with GetInteriorVehicleDataConsent(allowed=true) for all moduleIds to the app
- NOT transfer GetInteriorVehicleDataConsent to HMI
- NOT save driver's decision to cache

2.
In case
`accessMode` is set to `AUTO_ALLOW`  
and SDL receives GetInteriorVehicleDataConsent request from mobile app with 

- valid `moduleIds` with `userLocation` within `serviceArea` of each requested module or it is driver location
- (allowMultipleAccess = false)

SDL must
- respond with `allowed=true` for all free `moduleIds` to the app
- respond with `allowed=false` for all allocated `moduleIds` to the app
- NOT transfer GetInteriorVehicleDataConsent request to HMI
- NOT save driver's decision to cache

### AUTO_DENY

1.
In case  
`accessMode` is set to `AUTO_DENY`  
and SDL receives GetInteriorVehicleDataConsent request from mobile app with 

- valid `moduleIds` with `userLocation` within `serviceArea` of each requested module or it is driver location

SDL must
- respond with `allowed=true` for all free `moduleIds` to the app
- respond with `allowed=false` for all allocated `moduleIds` to the app
- NOT transfer GetInteriorVehicleDataConsent request to HMI
- NOT save driver's decision to cache

### ASK_DRIVER

1.
In case  
`accessMode` is set to `ASK_DRIVER` on HMI  
SDL receives GetInteriorVehicleDataConsent request from mobile app (in HMILevel FULL) with

- valid `moduleIds` with `userLocation` within `serviceArea` of each requested modules or it is driver location
- (allowMultipleAccess=true)
- the requested modules are already allocated to another RC application

and SDL doesn't have internally cached concent value

SDL must

- transfer GetInteriorVehicleDataConsent_request to HMI  
- respond with received GetInteriorVehicleDataConsent_response from HMI to mobile  
- save driver's decision to cache

2.
In case  
SDL requested user consent from HMI via GetInteriorVehicleDataConsent_request  
and user disallowed access to RC module for the requested application

SDL must

- respond on control request to RC application with result code REJECTED, success:false, info: "The resource is in use and the driver disallows this remote control RPC"
- NOT allocate access for remote control module to the requested application (meaning SDL must leave control of remote control module without changes)

_Note: All further requests from this application to the same module in case if it is still under control of another application must be rejected by SDL without initiating consent prompts_

3.
In case 
`accessMode` is set to `ASK_DRIVER` on HMI
SDL received GetInteriorVehicleDataConsent request from mobile app (in HMILevel FULL) with

- valid `moduleIds` with `userLocation` within `serviceArea` of each requested modules or it is driver location
- (allowMultipleAccess=true)
- the requested modules are already allocated to another RC application

and SDL has internally cached concent value

SDL must
- respond with the cached value to the app
- NOT transfer GetInteriorVehicleDataConsent_request to HMI

4.
In case   
`accessMode` is set to `ASK_DRIVER` on HMI  
SDL received GetInteriorVehicleDataConsent request from mobile with

- valid `moduleIds` with `userLocation` within `serviceArea` of each requested modules or it is driver location
- (allowMultipleAccess=true)
- the requested modules are free

SDL must
- respond with GetInteriorVehicleDataConsent(allowed=true) for all moduleIds to the app
- NOT transfer GetInteriorVehicleDataConsent to HMI
- NOT save driver's decision to cache

5.
In case  
`accessMode` is set to `ASK_DRIVER` on HMI  
SDL received GetInteriorVehicleDataConsent request from mobile app (in HMILevel FULL) with

- valid `moduleIds` with `userLocation` out of `serviceArea` of each requested modules
- (allowMultipleAccess = true)

SDL must
- respond with GetInteriorVehicleDataConsent(allowed:false) to the app
- NOT transfer GetInteriorVehicleDataConsent_request to HMI

6.
In case  
`accessMode` is set to `ASK_DRIVER` on HMI  
SDL received GetInteriorVehicleDataConsent request from mobile app (in HMILevel FULL) with

- valid `moduleIds` with `userLocation` within `serviceArea` of each requested modules
- (allowMultipleAccess = false)
- the requested modules are already allocated to another RC application

SDL must
- respond with `allowed=false` for all allocated `moduleIds` to the app
- NOT transfer GetInteriorVehicleDataConsent request to HMI
- NOT save driver's decision to cache

7.
In case  
SDL requested user consent from HMI via GetInteriorVehicleDataConsent_request  
and user did not provide the answer during default timeout  
and HMI sends GetInteriorVehicleDataConsent(TIMED_OUT) to SDL

SDL must

- respond on control request to RC application with result code TIMED_OUT, success:false, info: "The resource is in use and the driver did not respond in time"  
- NOT allocate access for remote control module to the requested application (meaning SDL must leave control of remote control module without changes)

_Note: SDL must initiate user prompt in case of consequent control request for the same module from this application_

8.
In case  
SDL requested user consent from HMI via GetInteriorVehicleDataConsent_request  
and HMI did not respond during default timeout or response is invalid or erroneous

SDL must

- respond on control request to RC application with result code GENERIC_ERROR, success:false  
- NOT allocate access for remote control module to the requested application (meaning SDL must leave control of remote control module without changes)

_Note: SDL must initiate user prompt in case of consequent control request for the same module from this application_


## Non-functional Requirements
### Mobile API
```
<interface name="RC" version="2.0.0" date="2018-09-05">
    <function name="GetInteriorVehicleDataConsent" functionID="GetInteriorVehicleDataConsentID" messagetype="request" since="6.0">
        <param name="moduleType" type="ModuleType" mandatory="true">
            <description>The module type that the app requests to control.</description>
        </param>
        <param name="moduleIds" type="String" maxlength="100" array="true" mandatory="true">
            <description>Ids of a module of same type, published by System Capability. </description>
       </param>
    </function>

    <function name="GetInteriorVehicleDataConsent" functionID="GetInteriorVehicleDataConsentID" messagetype="response" since="6.0">
        <param name="allowed" type="Boolean" array="true" mandatory="false">
            <description>This array has the same size as "moduleIds" in the request; each element corresponding to one moduleId  
            "true" - if SDL grants the permission for the requested module;
            "false" - SDL denies the permission for the requested module.</description>
        </param>
        <param name="resultCode" type="Result" platform="documentation" mandatory="true">
            <description>See Result</description>
            <element name="SUCCESS"/>
            <element name="INVALID_DATA"/>
            <element name="OUT_OF_MEMORY"/>
            <element name="TOO_MANY_PENDING_REQUESTS"/>
            <element name="APPLICATION_NOT_REGISTERED"/>
            <element name="GENERIC_ERROR"/>
            <element name="REJECTED"/>
            <element name="IGNORED"/>
            <element name="DISALLOWED"/>
            <element name="USER_DISALLOWED"/>
            <element name="UNSUPPORTED_RESOURCE"/>
        </param>
        <param name="info" type="String" maxlength="1000" mandatory="false">
        </param>
        <param name="success" type="Boolean" platform="documentation" mandatory="true">
            <description> true if successful; false, if failed </description>
        </param>
    </function>
```

## Diagrams

GetInteriorVehicleDataConsent
![GetInteriorVehicleDataConsent](https://github.com/smartdevicelink/sdl_requirements/blob/master/detailed_docs/accessories/GetInteriorVehicleDataConsent.png)

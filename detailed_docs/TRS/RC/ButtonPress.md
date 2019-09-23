## Functional Requirements

1.
In case
- application registered with REMOTE_CONTROL AppHMIType
sent ButtonPress RPC allowed by app's policies with
"moduleType" and "buttonName" related to one and the same module
- and signed moduleType is available on HMI
- and signed moduleType is allowed for this application

SDL must
- transfer this request to HMI
- respond with <resultCode> received from HMI
- send OnRCStatus to HMI with the module in parameter `allocatedModules`
and remained modules in `freeModules`
- send OnRCStatus to the application with the module in parameter `allocatedModules`
and remained modules in `freeModules` and with allowed:true

2.
In case

application registered with REMOTE_CONTROL AppHMIType
sent ButtonPress RPC with
`buttonName` "related-to-module_X" and `moduleType` + `moduleID` "module_Y"

SDL must

- respond with "resultCode: INVALID_DATA, success: false" to this mobile application
- not transfer this request to HMI

3.
In case
- application registered with REMOTE_CONTROL AppHMIType
sent ButtonPress RPC
- and HMI responds with ButtonPress(resultCode: READ_ONLY) 

SDL must

- respond with "resultCode: GENERIC_ERROR, success: false" to this mobile application

4.
In case
- application registered with REMOTE_CONTROL AppHMIType
sends valid ButtonPress RPC allowed by app's policies with omitted `moduleID` param

SDL must
- add the default value of `moduleID` param (the `moduleId` of first module of requested `moduleType` provided in RC capabilities from HMI)
- transfer the request to HMI
- respond with <resultCode> received from HMI

5.
In case
- application registered with REMOTE_CONTROL AppHMIType
sends valid ButtonPress RPC allowed by app's policies with 
- incorrect value of `moduleID` param or with the value of incorrect data type 

SDL must
- NOT transfer the request to HMI
- respond with UNSUPPORTED_RESOURCE to mobile app

6.
In case
- application registered with REMOTE_CONTROL AppHMIType
sends valid ButtonPress RPC allowed by app's policies with 
- correct value of `moduleID` param and incorrect values of mandatory params

SDL must
- NOT transfer the request to HMI
- respond with INVALID_DATA to mobile app

## Non-functional Requirements
### Mobile API
```
   <function name="ButtonPress" functionID="ButtonPressID" messagetype="request" since="4.5">
        <param name="moduleType" type="ModuleType" mandatory="true">
            <description>The module where the button should be pressed</description>
        </param>
        <param name="buttonName" type="ButtonName" mandatory="true">
            <description>The name of supported RC climate or radio button.</description>
        </param>
        <param name="buttonPressMode" type="ButtonPressMode" mandatory="true">
            <description>Indicates whether this is a LONG or SHORT button press event.</description>
        </param>
+       <param name="moduleId" type="String" maxlength="100" mandatory="false" since="5.x">
+           <description>Id of a module, published by System Capability. <description>
+       </param>
    </function>
```

## Diagram

ButtonPress

![ButtonPress](https://github.com/smartdevicelink/sdl_requirements/blob/master/detailed_docs/accessories/ButtonPress.png)

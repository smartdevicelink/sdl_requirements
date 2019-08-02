## Functional Requirements

1.
In case  
RC app has some allocated modules  
and sends valid ReleaseInteriorVehicleDataModule request with all mandatory params

SDL must
- respond ReleaseInteriorVehicleDataModule(SUCCESS) info:"Module is successfully released" to this app
- send OnRCStatus(`freeModules`) notifications to HMI
- send OnRCStatus(`freeModules`) notifications to the app

2.
In case  
app sends ReleaseInteriorVehicleDataModule(`module_type`, `module_id`) request to release a free module

SDL must  
respond with result_code=IGNORED with info info="This module is not allocated for this application") to the app

3.
In case  
app sends ReleaseInteriorVehicleDataModule(`module_type`, `module_id`) request to release a module allocated by another application

SDL must  
respond with result_code=REJECTED with info info="This module is allocated by different application") to the app

4.
In case 
app sends ReleaseInteriorVehicleDataModule request to release non-existing RC module type

SDL must  
respond with resultCode:"INVALID_DATA" to the app

## Non-functional Requirements
### Mobile API 

```xml
<function name="ReleaseInteriorVehicleDataModule" functionID="ReleaseInteriorVehicleDataModuleID" messagetype="request" since="5.x">
    <param name="moduleType" type="ModuleType" mandatory="true">
    </param>
    <param name="moduleId" type="String" maxlength="100" mandatory="false" since="5.x">
       <description>Id of a module, published by System Capability. <description>
    </param>
</function>

<function name="ReleaseInteriorVehicleDataModule" functionID="ReleaseInteriorVehicleDataModuleID" messagetype="response" since="5.x">
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
        <element name="UNSUPPORTED_RESOURCE"/>
    </param>
    <param name="info" type="String" maxlength="1000" mandatory="false">
    </param>
    <param name="success" type="Boolean" platform="documentation" mandatory="true">
        <description> true if successful; false, if failed </description>
    </param>
</function>
```

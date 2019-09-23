## Functional Requirements

1.
In case 

mobile application sends valid  

and allowed by Policies SetGlobalProperties(`userLocation`, `appID`) request to SDL

SDL must 
- transfer RC.SetGlobalProperties_request to HMI
- save internally `userLocation` for the `appID`
- respond with `<resultCode>` received from HMI to mobile application

2.   
In case 

mobile application sends an SetGlobalProperties_request  

that is NOT included (omitted) in the PolicyTable group(s) assigned to the application  

SDL must 

return DISALLOWED, success:false to this mobile app

3.
In case 

mobile application sends invalid request

SDL must 

respond with resultCode "INVALID_DATA" and success:"false" to this mobile app

4.
In case 

mobile application sends SetGlobalProperties_request without `userLocation` param

SDL must
- consider app location as a `DRIVER` by default
- retrieve driver's location from `seatLocationCapabilities` (the fisrt element published by hmi)
- save internally `userLocation` for the `appID`

## Non-Functional Requirements
### Mobile API

```xml
<function name="SetGlobalProperties" functionID="SetGlobalPropertiesID" messagetype="request" since="1.0">
    ...
    <param name="userLocation" type="SeatLocation" mandatory="false" since="x.x">
        <description>Location of the user's seat. Default is driver's seat location if it is not set yet.</description>
    </param>
</function>

<enum name="GlobalProperty" since="1.0">
    ...
    <element name="USER_LOCATION" since="x.x">
        <description>Location of the user's seat of setGlobalProperties</description>
    </element>
</enum>
```

### HMI_API

```xml 
<function name="SetGlobalProperties" messagetype="request">
    <description>Sets some properties for the application initiated request.</description>
    <param name="userLocation" type="SeatLocation" mandatory="false">
        <description>Location of the user's seat. Default is driver's seat location if it is not set yet.</description>
    </param>
    <param name="appID" type="Integer" mandatory="true">
        <description>ID of application related to this RPC.</description>
    </param>
</function>
<function name="SetGlobalProperties" messagetype="response">
</function>
```

## Diagram
RC_SetGlobalProperties
![RC_SetGlobalProperties](https://github.com/smartdevicelink/sdl_requirements/blob/master/detailed_docs/accessories/RC_SetGlobalProperties.png)

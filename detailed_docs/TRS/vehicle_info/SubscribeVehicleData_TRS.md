## Functional Requirements

1.
In case mobile application sends valid and allowed by Policies SubscribeVehicleData_request to SDL

SDL must: 
- transfer SubscribeVehicleData_request to HMI
- respond with `<resultCode>` received from HMI to mobile app 

2.
In case mobile application sends valid SubscribeVehicleData_request to SDL

and this request is NOT allowed by Policies

SDL must: 

respond "DISALLOWED, success:false" to mobile application

## Non-Functional Requirements
1.
```
<function name="SubscribeVehicleData" functionID="SubscribeVehicleDataID" messagetype="request">
            :
    <param name="fuelRange" type="Boolean" mandatory="false">
        <description>The estimate range in KM the vehicle can travel based on fuel level and consumption</description>
    </param>
</function>
<function name="SubscribeVehicleData" functionID="SubscribeVehicleDataID" messagetype="response">
            :
    <param name="fuelRange" type="VehicleDataResult" mandatory="false">
        <description>The estimate range in KM the vehicle can travel based on fuel level and consumption</description>
    </param>
</function>

```
2.
```
<function name="SubscribeVehicleData" functionID="SubscribeVehicleDataID" messagetype="request">
            :
    <param name="engineOilLife" type="Boolean" mandatory="false">
        <description>The estimated percentage of remaining oil life of the engine.</description>
    </param>
</function>
<function name="SubscribeVehicleData" functionID="SubscribeVehicleDataID" messagetype="response">
            :
    <param name="engineOilLife" type="VehicleDataResult" mandatory="false">
        <description>The estimated percentage of remaining oil life of the engine.</description>
    </param>
</function>
```
## Diagrams

SubscribeVehicleData

![]()

## Functional Requirements

1.
In case mobile application sends valid UnsubscribeVehicleData_request to SDL to unsubscribe already subscribed parameters 
- and this request is allowed by Policies
- and there are no other applications subscribed on vehicle info notifications

SDL must: 
- transfer UnsubscribeVehicleData_request to HMI
- respond with `<resultCode>` received from HMI to mobile application


2.
In case SDL successfully subscribes param_1 for app_1 and app_2 via SubscribeVehicleData 

and app_1 sends UnsubscribeVehicleData (param_1)  

SDL must:  
- respond via UnsubscribeVehicleData(SUCCESS) to app_1  
- stop sending OnVehicleData (param_1) ONLY to app_1  
- NOT send UnsubscribeVehicleData (param_1) to HMI while there are app_2 still subscribed on param_1  

3.
In case mobile application sends an UnsubscribeVehicleData_request that is NOT included (omitted) in the PolicyTable group(s) assigned to the application  

SDL must:  
return DISALLOWED, success:false to this mobile app  

4.

## Non-Functiona Requirements
```
<function name="UnsubscribeVehicleData" functionID="UnsubscribeVehicleDataID" messagetype="request">
            :
    <param name="fuelRange" type="Boolean" mandatory="false">
        <description>The estimate range in KM the vehicle can travel based on fuel level and consumption</description>
    </param>
</function>
<function name="UnsubscribeVehicleData" functionID="UnsubscribeVehicleDataID" messagetype="response">
            :
    <param name="fuelRange" type="VehicleDataResult" mandatory="false">
        <description>The estimate range in KM the vehicle can travel based on fuel level and consumption</description>
    </param>
</function>
<param name="fuelRange" type="FuelRange" minsize="0" maxsize="100" array="true" mandatory="false">
<function name="GetVehicleData" functionID="GetVehicleDataID" messagetype="request">
            :
    <param name="fuelRange" type="Boolean" mandatory="false">
        <description>The estimate range in KM the vehicle can travel based on fuel level and consumption</description>
    </param>
</function>
```

```
<function name="UnsubscribeVehicleData" functionID="UnsubscribeVehicleDataID" messagetype="request">
            :
    <param name="engineOilLife" type="Boolean" mandatory="false">
        <description>The estimated percentage of remaining oil life of the engine.</description>
    </param>
</function>
<function name="UnsubscribeVehicleData" functionID="UnsubscribeVehicleDataID" messagetype="response">
            :
    <param name="engineOilLife" type="VehicleDataResult" mandatory="false">
        <description>The estimated percentage of remaining oil life of the engine.</description>
    </param>
</function>
```

## Diagram

UnsubscribeVehicleData

![UnsubscribeVehicleData](.png)

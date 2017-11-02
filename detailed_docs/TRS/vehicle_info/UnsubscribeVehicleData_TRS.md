## Functional Requirements

1.
In case mobile application sent valid UnsubscribeVehicleData_request to SDL
- and this request is allowed by Policies
- and there are no other applications subscribed on vehicle info npotifications

SDL must: 
- transfer UnsubscribeVehicleData_request to HMI
- respond with `<resultCode>` received from HMI to mobile application

2.
In case mobile application sent valid UnsubscribeVehicleData_request to SDL
- and this request is allowed by Policies
- and there are other applications still subscribed on vehicle info npotifications

SDL must: 
- unsubscribe requesting application from vehicle data change notifications
- not send UnsubscribeVehicleData_reques to HMI

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

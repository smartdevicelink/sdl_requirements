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
In case mobile application sends UnsubscribeVehicleData_request  

that is NOT included (omitted) in the PolicyTable group(s) assigned to the application  

SDL must:  
return DISALLOWED, success:false to this mobile app  

4.  
In case mobile application is NOT subscribed for a single or multiple `<vehicleData>`

and sends UnsubscribeVehicleData_request with all `<vehicleData>` not subscribed before  

SDL must:  
- respond with (IGNORED, success:false {<vehicleData>: "DATA_NOT_SUBSCRIBED"} ) to mobile app  

5.
In case UnubscribeVehicleData request is trying to unsubscribe the parameter the application is not yet subscribed for (even if the request contains at least one subscribed parameter)  

SDL must: 

- ignore not subscribed items and transfer subscribed params of UnsubscribeVehicleData to HMI  
- get the response with general_result_Code_from_HMI and newly-unsubscribed parameters with their correponding individual-result-codes from HMI  
- respond to mobile application with "ResultCode: IGNORED, success: <applicable flag>" + "info" parameter+ all parameters and their correponding individual result codes got from HMI and all ignored parameter(s) with the individual result code(s): DATA_NOT_SUBSCRIBED for items ignored by SDL

_Important Note: success:true and resultCode: IGNORED is applicabe for general_result_Code_from_HMI SUCCESS/WARNINGS, otherwise success:false and resultCode: general_result_Code_from_HMI must be returned to mobile application._

6.  
In case UnsubscribeVehicleData RPC is allowed by policies with less than supported by protocol parameters  

and the app assigned with such policies requests UnsubscribeVehicleData with one and-or more NOT-allowed params only  

SDL must:  

- send nothing to HMI  
- return the individual results of DISALLOWED to response to mobile application + "ResultCode:DISALLOWED, success: false"  

7.
In case mobile app sends SubscribeVehicleData_request to SDL  

and this request is allowed by Policies for this mobile application  

and "parameters" field is **empty** at PolicyTable for SubscribeVehicleData RPC  

SDL must: 

- respond with "DISALLOWED, success:false", "info: Requested parameters are disallowed by Policies" 
- NOT transfer this request to HMI

8.  
	
In case mobile application sends UnsubscribeVehicleData_request to SDL  

and this request is allowed by Policies for this mobile application  

and "parameters" field is **omitted** at PolicyTable for this request  

SDL must:  
- transfer received request with all requested parameters as is to HMI  
- respond with `<received_resultCode_from_HMI>` to mobile app


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

![UnsubscribeVehicleData](https://github.com/smartdevicelink/sdl_requirements/tree/feature/FuelRange/detailed_docs/accessories/UnsubscribeVehicleData.png)

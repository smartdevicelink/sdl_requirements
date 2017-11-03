## Functional Requirements

1.
In case app_1 sends SubscribeVehicleData (param_1) to SDL   

and SDL does not have this param_1 in list of stored successfully subscribed params  

SDL must:  

- transfer SubscribeVehicleData(param_1) to HMI
- in case SDL receives SUCCESS from HMI for param_1  
SDL must store this param in list 
- respond with corresponding result SUCCESS received from HMI to app_1

2.  
In case app_1 sends SubscribeVehicleData (param_1) to SDL  

and SDL does not have this param_1 in list of stored successfully subscribing params  

SDL must:  
- transfer SubscribeVehicleData(param_1) to HMI  
- in case SDL receives erroneous result from HMI for param_1  
SDL must NOT store the parameter in SDL list   
- respond SubscribeVehicleData (Result Code, success:false) to app_1

3.  
In case app_2 sends SubscribeVehicleData (param_1, param_2) to SDL  

and  SDL has already successfully subscribed app_1 to param_1 and param_2  

SDL must:  

- NOT send SubscribeVehicleData(param_1, param_2) to HMI  
- respond via SubscribeVehicleData (SUCCESS) to app_2  

4.
In case mobile application sends UnsubscribeVehicleData_request that is NOT included (omitted) in the PolicyTable group(s) assigned to the application  

SDL must:  
return DISALLOWED, success:false to this mobile app  

5.  
In case mobile application already subscribed on a single or multiple `<vehicleData>`  

and sends SubscribeVehicleData_request with all `<vehicleData>` subscribed by the application before  

SDL must:  

- not transfer a resuest to HMI  
- respond with (IGNORED, success:false {`<vehicleData>`: DATA_ALREADY_SUBSCRIBED} ) to mobile app 

6.  
In case mobile application sends valid SubscribeVehicleData_request 

trying to subscribe the parameter the application has already subscribed for (even if the request contains at least one non-subscribed parameter)

SDL must:  

- ignore alredy subscribed items and transfer not subscribed params of SubscribeVehicleData to HMI
- get the response with general_result_Code_from_HMI and newly-subscribed parameters with their correponding individual-result-codes from HMI 
- respond to mobile application with "ResultCode: IGNORED, success: `<applicable flag>`" + "info" parameter+ all parameters and their correponding individual result codes got from HMI and all ignored parameter(s) with the individual result code(s): DATA_ALREADY_SUBSCRIBED for items ignored by SDL  

_Important Note: success:true and resultCode: IGNORED is applicabe for general_result_Code_from_HMI SUCCESS/WARNINGS, otherwise success:false and resultCode: general_result_Code_from_HMI must be returned to mobile application._  

7.  
	
In case SubscribeVehicleData RPC is allowed by Policies with less than supported by protocol parameters  

and the app assigned with such policies requests SubscribeVehicleData with one and-or more NOT-allowed params only  

SDL must:  

- send nothing to HMI  
- return the individual results of DISALLOWED to response to mobile application + "ResultCode:DISALLOWED, success: false"  

8.
In case mobile app sends SubscribeVehicleData_request to SDL  

and this request is allowed by Policies for this mobile application  

and "parameters" field is **empty** at PolicyTable for SubscribeVehicleData RPC  

SDL must:

- respond with "DISALLOWED, success:false", "info: Requested parameters are disallowed by Policies" 
- NOT transfer this request to HMI  

9.
In case mobile application sends UnsubscribeVehicleData_request to SDL  

and this request is allowed by Policies for this mobile application  

and "parameters" field is **omited** at PolicyTable for this request  

SDL must:  
- transfer received request with all requested parameters as is to HMI  
- respond with `<received_resultCode_from_HMI>` to mobile app


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

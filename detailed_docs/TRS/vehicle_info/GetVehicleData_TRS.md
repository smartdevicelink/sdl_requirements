## Functional Requirements

1.
In case mobile application sends valid  

and allowed by Policies GetVehicleData_request to SDL

SDL must: 
- transfer GetVehicleData_request to HMI
- respond with `<resultCode>` received from HMI to mobile application

2.   
In case mobile application sends an GetVehicleData_request  

that is NOT included (omitted) in the PolicyTable group(s) assigned to the application  

SDL must:  
return DISALLOWED, success:false to this mobile app

3.
In case mobile application sends GetVehicleData_request to SDL  
with one and-or more allowed params and with one and-or more NOT-allowed params by Policies  

SDL must:  
- transfer GetVehicleData_request to HMI with allowed params only  
- ignore the NOT-allowed params  
- respond to mobile application transfered vehicleData values from HMI with "ResultCode: `<applicable-result-code>`, success: `<applicable flag>`" + "info" parameter listing the params disallowed by policies   

4. 
In case mobile application sends GetVehicleData_request to SDL

and this request is allowed by Policies for this mobile application

and "parameters" field is **empty** in PolicyTable for GetVehicleData RPC

SDL must:  
respond with "DISALLOWED, success:false", "info: Requested parameters are disallowed by Policies" to mobile application  

6.
In case mobile application sends request to SDL  

and this request is allowed by Policies for this mobile app  

and "parameters" field is **omitted** at PolicyTable for this request  

SDL must:  
- transfer received request with all requested parameters as is to HMI  
- respond with `<received_resultCode_from_HMI>` to mobile app  

7. In case an application sends a request and:
-> either unknown issue happenes 
-> or something goes wrong  

SDL must:

respond with "GENERIC_ERROR, success:"false" to mobile application.


## Non-Functional Requirements
Additions to mobile API, HMI_API  
Changes to enums and functions should be applied  
1.

```xml
<enum name="FuelType">
    <element name="GASOLINE" />
    <element name="DIESEL" />
    <element name="CNG">
        <description>
            For vehicles using compressed natural gas.
        </description>
    </element>
    <element name="LPG">
        <description>
            For vehicles using liquefied petroleum gas.
        </description>
    </element>
    <element name="HYDROGEN">
        <description>For FCEV (fuel cell electric vehicle).</description>
    </element>
    <element name="BATTERY">
        <description>For BEV (Battery Electric Vehicle), PHEV (Plug-in Hybrid Electric Vehicle), solar vehicles and other vehicles which run on a battery.</description>
    </element>
</enum>

<struct name="FuelRange">
    <param name="type" type="FuelType" mandatory="false"/>
    <param name="range" type="Float" minvalue="0" maxvalue="10000" mandatory="false">
        <description>
            The estimate range in KM the vehicle can travel based on fuel level and consumption.
        </description>
    </param>
</struct>

<enum name="VehicleDataType">
            :
    <element name="VEHICLEDATA_FUELRANGE" />
</enum>


<function name="GetVehicleData" functionID="GetVehicleDataID" messagetype="response">
            :
    <param name="fuelRange" type="FuelRange" minsize="0" maxsize="100" array="true" mandatory="false">
        <description>The estimate range in KM the vehicle can travel based on fuel level and consumption</description>
    </param>
</function>
```
2.
```
<function name="GetVehicleData" functionID="GetVehicleDataID" messagetype="request">
            :
	<param name="engineOilLife" type="Boolean" mandatory="false">
        <description>The estimated percentage of remaining oil life of the engine.</description>
    </param>
</function>
<function name="GetVehicleData" functionID="GetVehicleDataID" messagetype="response">
            :
    <param name="engineOilLife" type="Float" minvalue="0" maxvalue="100" mandatory="false">
        <description>The estimated percentage of remaining oil life of the engine.</description>
    </param>
</function>
```

## Diagram

## Functional Requirements

1. 
In case mobile application has already subscribed on some vehicle data via SubscribeVehicleData_request  

and this subscribed data was updated  

SDL must:  

transfer OnVehicleData_notification (with updated vehicle data value) to subscribed mobile applications  

2.  
In case HMI sends invalid notification that SDL should transfer to mobile app  

SDL must:  

- log the issue  
- ignore this notification  

Invalid notifications means the notification contains:  
a. params out of bounds or incorrect symbols (/t, /n, `<whitespace>`)  
b. mandatory params are missing  
c. params of wrong type  
d. invalid json  
e. incorrect combination of params


3. 
In case SDL received valid OnVehicleData notification from HMI

and this notification is NOT allowed by Policies

SDL must:

- log corresponding error internally
- ignore this notification and not transfer it to subscribed mobile applications  

4.
In case SDL receives OnVehicleData notification from HMI  

and this notification is allowed by Policies for this mobile app  

and "parameters" field is omited at PolicyTable for this notification  

SDL must:  
- transfer received notification with all parameters as is to mobile app  
- respond with `<received_resultCode_from_HMI>` to mobile app


## Non-Functional Requirements  
Additions to mobile API, HMI_API  
Changes to enums and functions should be applied  

1.
```
<function name="OnVehicleData" functionID="OnVehicleDataID" messagetype="notification">
            :
    <param name="fuelRange" type="FuelRange" minsize="0" maxsize="100" array="true" mandatory="false">
        <description>The estimate range in KM the vehicle can travel based on fuel level and consumption</description>
    </param>
</function>
```
```xml
<enum name="VehicleDataType">
            :
    <element name="VEHICLEDATA_ENGINEOILLIFE" />
</enum>
```
2.
```
<function name="OnVehicleData" functionID="OnVehicleDataID" messagetype="notification">
            :
    <param name="engineOilLife" type="Float" minvalue="0" maxvalue="100" mandatory="false">
        <description>The estimated percentage of remaining oil life of the engine.</description>
    </param>
</function>
```


## Diagram

OnVehicleData

![](.png)



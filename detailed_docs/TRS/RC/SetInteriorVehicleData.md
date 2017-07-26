## Functional Requirements

1.
In case

application is registered with REMOTE_CONTROL appHMIType
sent valid SetInteriorVehicleData RPC with <"moduleType"> and "<*ControlData>" parameters

SDL must
- transfer this request to HMI
- respond with <result_code> received from HMI

2. 
In case
- application is registered with REMOTE_CONTROL appHMIType
sent valid SetInteriorVehicleData RPC with <"moduleType"> and <"*ControlData"> parameters
- requested "moduleType" is unavalable on HMI

SDL must
- not transfer this request to HMI
- respond with UNSUPPRTED_RESOURCE, success:false result code to mobile app

3. 
In case 

application is registered with REMOTE_CONTROL AppHMIType 
sent SetInteriorVehicleData RPC with 
"ControlData" for "module_X" and moduleType "module_Y"

SDL must 
- respond with "resultCode: INVALID_DATA, success: false" to this mobile app
- not transfer this request to HMI

4.
In case 

application sent valid SetInteriorVehicleData with just read-only parameters in "*ControlData" struct for the appropriate muduleType 

SDL must 

respond with "resultCode: READ_ONLY, success:false" to this application and not process this RPC. 

5. 
In case 

application sent valid SetInteriorVehicleData with read-only parameters and one or more settable parameters in "*ControlData" struct for the appropriate moduleType 

SDL must 

cut off read-only parameters and process this RPC as assigned (that is, check policies, send to HMI, and etc. per existing requirements)

6. 
In case 
- application sent SetInteriorVehicleData with one or more settable parameters in "*ControlData" struct, for the appropriate muduleType 
- and HMI responds with "resultCode: READ_ONLY" 

SDL must 

send "resultCode: READ_ONLY, success:false" to the related mobile application. 

7.
In case

mobile app sent SetInteriorVehicleData with all unknown controlData itmes from *controlData structure  or all controlData itmes that do not match with moduleType (for example app requested SetInteriorVehicledata for RADIO module with ClimateControlData items only),

SDL must
- respond INVALID_DATA success:false to mobile app
- not transfer this request to HMI

8. 
In case
- mobile app sent SetInteriorVehicleData with controlData structure that matches with moduleType
- and control data structure has unknown parameters or control items related to another moduleType,

SDL must
- cut off these fake parameters
- forward to HMI only items of controlData structure that are related to requested moduleType.

9. 
In case 

SDL gets response from HMI with (resultCode: READ_ONLY) on <"any-RPC-except-of-SetInteriorVehicleData"> 

SDL must 

send <"any-RPC-except-of-SetInteriorVehicleData"> response (resultCode: GENERIC_ERROR, success: false) to the related mobile app. 

## Diagrams

SetInteriorVehicleData

![SetInteriorVehicleData](https://github.com/smartdevicelink/sdl_requirements/blob/SetInteriorVehicleData_TRS/detailed_docs/accessories/SetInteriorVehicleData.png)

SetInteriorVehicleData_READ_ONLY
![SetInteriorVehicleData_READ_ONLY](https://github.com/smartdevicelink/sdl_requirements/blob/SetInteriorVehicleData_TRS/detailed_docs/accessories/SetInteriorVehicleData_READ_ONLY.png)

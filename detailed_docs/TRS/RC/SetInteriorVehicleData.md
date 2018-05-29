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

## Non-Functional requirements

1. The list of control items considered in the each control module

| RC Module | Control Item | Value Range |Type | Comments |
| ------------ | ------------ |------------ | ------------ | ------------ |
| Radio | Radio Enabled | true,false  | Get/Set/Notification| all other radio control items need radio enabled to work|
|       | Radio Band | AM,FM,XM  | Get/Set/Notification| |
|       | Radio Frequency | | Get/Set/Notification | value range depends on band |
|       | Radio RDS Data | | Get/Notification | read only |
|       | Available HD Channel | 1-3 | Get/Notification | read only |
|       | Current HD Channel | 1-3 | Get/Set/Notification |
|       | Radio Signal Strength |  | Get/Notification | read only |
|       | Signal Change Threshold |  | Get/Notification | read only |
|       | Radio State | Acquiring, acquired, multicast, not_found | Get/Notification | read only |
| Climate | Current Cabin Temperature |  | Get/Notification | read only, value range depends on OEM |
|         | Desired Cabin Temperature |  | Get/Set/Notification | value range depends on OEM |
|         | AC Setting | on,off | Get/Set/Notification |  |
|         | AC MAX Setting | on,off  | Get/Set/Notification |  |
|         | Air Recirculation Setting | on,off  | Get/Set/Notification |  |
|         | Auto AC Mode Setting | on,off  | Get/Set/Notification |  |
|         | Defrost Zone Setting | front,rear,all,none  | Get/Set/Notification |  |
|         | Dual Mode Setting | on,off  | Get/Set/Notification |  |
|         | Fan Speed Setting | 0%-100% | Get/Set/Notification |  |
|         | Ventilation Mode Setting | upper,lower,both,none  | Get/Set/Notification |  |  
| Seat    | | | | |
|         | Seat Heating Enabled | true, false | Get/Set/Notification | Indicates whether heating is enabled for a seat | |
|         | Seat Cooling Enabled | true, false | Get/Set/Notification | Indicates whether cooling is enabled for a seat | |
|         | Seat Heating  level | 0-100% | Get/Set/Notification | Level of the seat heating | |
|         | Seat Cooling  level | 0-100% | Get/Set/Notification | Level of the seat cooling | |
|         | Seat Horizontal Positon | 0-100% | Get/Set/Notification | adjust a seat forward/backward, 0 means the nearest position to the steering wheel, 100% means the furthest position from the steering wheel | |
|         | Seat Vertical Position | 0-100% | Get/Set/Notification | adjust seat height (up or down) in case there is only one actuator for seat height, 0 means the lowest position, 100% means the highest position | |
|         | Seat-Front Vertical Position | 0-100% | Get/Set/Notification | adjust seat front height (in case there are two actuators for seat height), 0 means the lowest position, 100% means the highest position | |
|         | Seat-Back Vertical Position | 0-100% | Get/Set/Notification | adjust seat back height (in case there are two actuators for seat height), 0 means the lowest position, 100% means the highest position | |
|         | Seat Back Tilt Angle | 0-100% | Get/Set/Notification | backrest recline, 0 means the angle that back top is nearest to the steering wheel, 100% means the angle that back top is furthest from the steering wheel | |
|         | Head Support Horizontal Position | 0-100% | Get/Set/Notification | adjust head support forward/backward, 0 means the nearest position to the front, 100% means the furthest position from the front | |
|         | Head Support Vertical Position | 0-100% | Get/Set/Notification | adjust head support height (up or down), 0 means the lowest position, 100% means the highest position | |
|         | Seat Massaging Enabled | true, false | Get/Set/Notification | indicates whether massage is enabled for a seat | | 
|         | Massage Mode | List of Struct {MassageZone, MassageMode} | Get/Set/Notification | list of massage mode of each zone | |
|         | Massage Cushion Firmness | List of Struct {Cushion, 0-100%} | Get/Set/Notification | list of firmness of each massage cushion | |
|         | Seat memory | Struct{ id, label, action (SAVE/RESTORE/NONE)} | Get/Set/Notification | seat memory | |


## Diagrams

SetInteriorVehicleData

![SetInteriorVehicleData](https://github.com/smartdevicelink/sdl_requirements/blob/master/detailed_docs/accessories/SetInteriorVehicleData.png)

SetInteriorVehicleData_READ_ONLY
![SetInteriorVehicleData_READ_ONLY](https://github.com/smartdevicelink/sdl_requirements/blob/master/detailed_docs/accessories/SetInteriorVehicleData_READ_ONLY.png)

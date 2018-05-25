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

send <"any-RPC-except-of-SetInteriorVehicleData"> response (resultCode: GENERIC_ERROR, success: false) to the related mobile app  

10. 
In case media mobile app is
1) RC and Media  
2) in FULL or LIMITED HMI level 
3) audio source=MOBILE_APP (keepContext = true)

and sends SetInteriorVehicleData request to change audio sourse to either itself (with target MOBILE_APP) or other system sources

SDL must  
- transfer this request to HMI  
- respond with <result_code> received from HMI  
- change audio source successfully  
- not change app's HMI level  

11. 
In case media mobile app is
1) RC and Media  
2) in FULL or LIMITED HMI level 
3) audio source=MOBILE_APP (keepContext = false) or without keepContext param  

SDL must  
- transfer this request to HMI  
- respond with <result_code> received from HMI  
- change audio source successfully
- change app's HMI level into 'BACKGROUND'


12. 
In case media mobile app is
1) RC and Media  
2) in BACKGROUND HMI level 
3) audio source=MOBILE_APP (keepContext = true)

and sends `Alert` (UI-related params WITH softButtons ‘yes’ ‘no’) to switch the audio source from others to itself  

SDL must  
- transfers the request to HMI with requested params displayed on the Alert dialog 
- receive from HMI OnHMIStatus(FULL, audible)  
- bring the application to foreground (HMI_FULL) 
- change audio source successfully

13.  
In case mobile app is  
1) RC and Non-Media  
2) in any HMI level  

and sends SetInteriorVehicleData to change audio sourse  

SDL must 
- transfer this request to HMI  
- respond with <result_code> received from HMI  
- not change audio source


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
|       | HD radio SIS data | see sisData type | Get,Notification | read only HD radio SIS data |
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
|         | heated windshield | true, false | Get/Set/Notification | true means ON, false means OFF | |
|         | heated rear window | true, false | Get/Set/Notification | true means ON, false means OFF | |
|         | heated steering wheel | true, false | Get/Set/Notification | true means ON, false means OFF | |
|         | heated mirrors | true, false | Get/Set/Notification | true means ON, false means OFF | |
| Audio   | | | | |  
|         | Audio volume | 0%-100% | Get/Set/Notification | the audio source volume level | |
|         | Audio Source | MOBILE_APP, RADIO_TUNER, CD, BLUETOOTH, USB, etc. see PrimaryAudioSource| Get/Set/Notification | defines one of the available audio sources | |
|         | keep Context | true, false | Set only | control whether HMI shall keep current application context or switch to default media UI/APP associated with the audio source | |
|         | Equalizer Settings | Struct {Channel ID as integer, Channel setting as 0%-100%} | Get/Set/Notification | defines the list of supported channels (band) and their current/desired settings on HMI | |  
| HMI Setting  | | | | |   
|         | Display Mode | DAY, NIGHT, AUTO | Get/Set/Notification | current display mode of the HMI display | |  
|         | Distance Unit | MILES, KILOMETERS | Get/Set/Notification | distance Unit used in the HMI (for maps/tracking distances) | |
|         | Temperature Unit | FAHRENHEIT, CELSIUS | Get/Set/Notification | temperature Unit used in the HMI (for temperature measuring systems) | |
| Light   | | | | | 
|         | Light Status | ON, OFF| Get/Set/Notification | turn on/off a single light or all lights in a group | |
|         | Light Density | float 0.0-1.0| Get/Set/Notification | change the density/dim a single light or all lights in a group| |
|         | Light Color | RGB color| Get/Set/Notification | change the color scheme of a single light or all lights in a group| |

## Diagrams

SetInteriorVehicleData

![SetInteriorVehicleData](https://github.com/smartdevicelink/sdl_requirements/blob/master/detailed_docs/accessories/SetInteriorVehicleData.png)

SetInteriorVehicleData_READ_ONLY
![SetInteriorVehicleData_READ_ONLY](https://github.com/smartdevicelink/sdl_requirements/blob/master/detailed_docs/accessories/SetInteriorVehicleData_READ_ONLY.png)

## Functional Requirements

1.
In case
- SDL received OnRemoteControlSettings notification from HMI with "ASK_DRIVER" access mode
- and RC application requested access to remote control module that is already allocated to another RC application
- and SDL requested user consent from HMI via GetInteriorVehicleDataConsent
- and user allowed access to RC module for requested application

SDL must
- allocate access to RC module to requested application
- process control request from this application

2.
In case
- SDL received OnRemoteControlSettings notification from HMI with "ASK_DRIVER" access mode
- and RC application requested access to remote control module that is already allocated to another RC application
- and SDL requested user consent from HMI via GetInteriorVehicleDataConsent
- and user did not provide the answer during default timeout
- and SDL received in response from HMI GetInteriorVehicleDataConsent (TIMED_OUT)

SDL must
- respond on control request to RC application with result code TIMED_OUT, success:false, info: "The resource is in use and the driver did not respond in time"
- not allocate access for remote control module to the requested application (meaning SDL must leave control of remote control module without changes)

*Note:* SDL must initiate user prompt in case of consequent control request for the same module from this application 

3.
In case
- SDL received OnRemoteControlSettings notification from HMI with "ASK_DRIVER" access mode
- and RC application requested access to remote control module that is already allocated to another RC application
- and SDL requested user consent from HMI via GetInteriorVehicleDataConsent
- and HMI did not respond during default timeout or response is invalid or erroneous

SDL must
- respond on control request to RC application with result code GENERIC_ERROR, success:false
- not allocate access for remote control module to the requested application (meaning SDL must leave control of remote control module without changes)

*Note:* SDL must initiate user prompt in case of consequent control request for the same module from this application 

4. 
In case
- SDL received OnRemoteControlSettings notification from HMI with "ASK_DRIVER" access mode
- and RC application requested access to remote control module that is already allocated to another RC application
- and SDL requested user consent from HMI via GetInteriorVehicleDataConsent
- and user disallowed access to RC module for the requested application

SDL must
- respond on control request to RC application with result code REJECTED, success:false, info: "The resource is in use and the driver disallows this remote control RPC"
- not allocate access for remote control module to the requested application (meaning SDL must leave control of remote control module without changes)

*Note:* All further requests from this application to the same module in case if it is still under control of another application must be rejected by SDL without initiating consent prompts

## Diagrams

GetInteriorVehicleDataConsent
![GetInteriorVehicleDataConsent](https://github.com/smartdevicelink/sdl_requirements/blob/GetInteriorVehicleDataConsent_TRS/detailed_docs/accessories/get_nterior_vehicle_data_consent.png)

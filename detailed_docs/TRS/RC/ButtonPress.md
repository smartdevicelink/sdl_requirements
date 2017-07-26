## Functional Requirements

1.
In case 
- application registered with REMOTE_CONTROL AppHMIType 
sent ButtonPress RPC allowed by app's policies with 
"moduleType" and "buttonName" related to one and the same module
- and signed moduleType is available on HMI
- and signed moduleType is allowed for this application

SDL must 
- transfer this request to HMI
- respond with <resultCode> received from HMI

2.
In case 

application registered with REMOTE_CONTROL AppHMIType 
sent ButtonPress RPC with 
<"buttonName-related-to-module_X> and moduleType "module_Y"

SDL must 

- respond with "resultCode: INVALID_DATA, success: false" to this mobile application
- not transfer this request to HMI

## Diagram

ButtonPress

![ButtonPress](https://github.com/smartdevicelink/sdl_requirements/blob/ButtonPress_TRS/detailed_docs/accessories/ButtonPress.png)

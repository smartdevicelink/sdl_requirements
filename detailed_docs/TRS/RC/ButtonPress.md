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
- send OnRCStatus to HMI with the module in parameter `allocatedModules`
and remained modules in `freeModules`
- send OnRCStatus to the application with the module in parameter `allocatedModules`
and remained modules in `freeModules` and with allowed:true

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

![ButtonPress](https://github.com/smartdevicelink/sdl_requirements/blob/master/detailed_docs/accessories/ButtonPress.png)

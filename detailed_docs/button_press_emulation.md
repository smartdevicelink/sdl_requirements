## Use Case 1: Button press event emulation from mobile app

**Main Flow:**

_Pre-conditions:_

a. mobile device is connected to SDL.

b. app is registered with REMOTE_CONTROL appHMIType with SDL from this device. 

c. app has "MODULE_1" moduleType assigned in its Policies

d. ModuleType "MODULE_1" is available on HMI for remote control

e. "MODULE_1" is not in control by another app

_Steps:_

1. Application requested to emulate Button Press of "ButtonName_for_MODULE_1" for "MODULE_1"

_Expected:_

2. HMI emulates Button Press of "ButtonName_MODULE_1" for "MODULE_1" successfully

**Exceptions:**

1.1 ButtonPress RPC is not valid -> SDL responds INVALID_DATA and doesn't process the request

1.2 ButtonPress RPC is not allowed by policies -> SDL responds DISALLOWED and doesn't process the request

1.3 Requested module is not allowed by policices -> SDL responds DISALLOWED and doesn't process the request

1.4 RC interface is not available on HMI -> SDL responds UNSUPPORTED_RESOURCE and doesn't process the request

1.5 Requested module is already in control by another app and control cannot be transferred to requested app -> SDL responds IN_USE and doesn't process the request

1.6 Requested module is busy with processing request from another application -> SDL responds IN_USE and doesn't process this request

**Alternative flow 1**

1.a.1 Application requested to emulate Button Press of "ButtonName_MODULE_2" for "MODULE_1"

_Expected:_

1.a.2 SDL does not process the request and returns INVALID_DATA, success:false to mobile app

> [#9](https://github.com/smartdevicelink/sdl_requirements/issues/9) [SDL_RC] Button press event emulation

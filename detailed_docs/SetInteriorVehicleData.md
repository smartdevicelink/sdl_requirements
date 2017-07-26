## Use Case 1: Set available control module settings

**Main Flow:**

_Pre-conditions:_

a. mobile device is connected to SDL.

b. app is registered with REMOTE_CONTROL appHMIType with SDL from this device.

c. moduleType_X is allowed for this app by policies

_Steps:_

1. Application requests to set available control settings for allowed moduleType_X

_Expected:_

2. SDL checks if such request is valid
3. SDL checks if such request is allowed by Policies for this app
4. SDL checks if signed in request moduleType_X is allowed by policies
5. SDL checks if signed in request moduleType_X is available on HMI
6. SDL checks if signed in request moduleType_X is controlled by another application
7. SDL checks *ControlData parameters
8. SDL transfers request to HMI
9. SDL receives the response from HMI
10. SDL transfers received response from HMI to the app
11. Requested settings were successfully changed on HMI according to application's request

**Exceptions:**

2.1 Request is invalid in case of wrong characters or symbols -> SDL returns INVALID_DATA and doesn't transfer the request to HMI

2.2 Request contains moduleType_X and *ControlData for moduleType_Y -> SDL returns INVALID_DATA and doesn't transfer the request to HMI

3.1 Request is disallowed by policies -> SDL returns USER_DISALLOWED and doesn't transfer the request to HMI

4.1 Requested moduleType_X is disallowed by policies -> SDL returns DISALLOWED and doesn't transfer the request to HMI

5.1 Requested moduleType_X is unavailable on HMI -> SDL returns UNSUPPORTED_RESOUCE and doesn't transfer the request to HMI

6.1 Requested module_Type_X is already controlled by another application -> SDL returns IN_USE and doesn't transfer the request to HMI

7.1 Application requested to set available control settings with read only control items -> SDL returns READ_ONLY result code and doesn't transfer the request to HMI

7.2 Application requested to set available control settings with read_only and settable control items -> SDL cuts off read_only control items and transfers only settable control items to HMI

7.3 Application requested to set available control settings for moduleType_X with all moduleType_Y control items in "*controlData" structure - > SDL returns INVALID_DATA result code and doesn't transfer this request to HMI

7.4 3 Application requested to set available control settings for moduleType_X with moduleType_Y or unknown control items and with moduleType_X control items -> SDL cuts off unknown or related to another module control items and transfers only related to requested moduleType control items to HMI

> Requirement: [#3](https://github.com/smartdevicelink/sdl_requirements/issues/3)[SDL_RC] Set available control module settings SetInteriorVehicleData

## Use Case 1: Current RC module status data request

**Main Flow:**

_Pre-conditions:_

a. device is connected to SDL.

b. app is registered with REMOTE_CONTROL appHMIType with SDL from this device. 

_Steps:_

1. App requests current RC module status data

_Expected:_

2. SDL checks if such request is valid
3. SDL checks if such request is allowed by Policies for such app
4. SDL transfers request to HMI
5. SDL receives the response from HMI
6. SDL transfers received response from HMI to the app
7. App receives current RC module status

_Exceptions:_

2.1 Request is invalid -> SDL returns INVALID_DATA, success:false

3.1 Requested moduleType is disallowed by policies -> SDL returns DISALLOWED, success:false

3.2 Request is disallowed by policies -> SDL returns USER_DISALLOWED, success:false

5.1 HMI didn't respond or received response is invalid -> SDL returns GENERIC_ERROR, success:false

5.2 HMI responds with errorCode -> SDL transfer received errorCode, success:false

> Requirement [#2](https://github.com/smartdevicelink/sdl_requirements/issues/2)

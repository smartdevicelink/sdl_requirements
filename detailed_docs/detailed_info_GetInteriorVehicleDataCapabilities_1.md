## Use Case 1: RC capabilities request of available modules and control items

**Main Flow:**

_Pre-conditions:_

a. device is connected to SDL.

b. app is registered with REMOTE_CONTROL appHMIType with SDL from this device. 

_Steps:_

1. App requests RC capabilities of available modules with corresponding control items

_Expected:_

2. SDL checks if such request is valid
3. SDL checks if such request is allowed by Policies for such app
4. SDL checks if requested modules are allowed by Policies
5. SDL transfers request to HMI
6. SDL receives the response from HMI
7. SDL transfers received response from HMI to the app
8. App receives current RC capabilities of available modules with corresponding control items

**Exceptions:**

2.1 Request is invalid -> SDL returns INVALID_DATA
3.1 Request is disallowed by policies -> SDL returns USER_DISALLOWED
4.1 Requested <moduleType> is disallowed by policies -> SDL returns DISALLOWED
6.1 HMI didn't respond or received response is invalid -> SDL returns GENERIC_ERROR
6.2 HMI responds with <errorCode> -> SDL transfer received <errorCode>, success:false

**Alternative flow 1**

6.a.1 One of the requested modules is not available on HMI

6.a.2 SDL transfers to mobile application capabilities of available on HMI module and responds with WARNINGS "%moduleType% is not available" for anavailable module 




> Requirement [#1](https://github.com/smartdevicelink/sdl_requirements/issues/1)

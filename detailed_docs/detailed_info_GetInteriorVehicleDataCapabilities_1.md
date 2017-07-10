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


**Alternative flow 1**

6.a.1 One of the requested modules is not available on HMI

6.a.2 SDL transfers to mobile application capabilities of available on HMI module and responds with WARNINGS "%moduleType% is not available" for anavailable module 


**Alternative flow 1**

6.b.1 HMI does not respond or HMI response is invalid

6.b.2 SDL transfers to mobile app capabilities for requested module from InteriorVehicleDataCapabilities.json file

**Exception:**

6.b.2.1  "InteriorVehicleDataCapabilities.json" file is invalid, empty or missing -> SDL must respond with "resultCode: GENERIC_ERROR, success: false, info: 'Invalid response from the vehicle'" to this mobile app's request

> Requirement [#1](https://github.com/smartdevicelink/sdl_requirements/issues/1)

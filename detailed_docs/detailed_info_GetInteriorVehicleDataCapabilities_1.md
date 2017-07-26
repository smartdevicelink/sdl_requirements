## Use Case 1: Mobile app requests RC capabilities

**Main Flow:**

_Pre-conditions:_

a. mobile app is registered with REMOTE_CONTROL appHMIType with SDL 

_Steps:_

1. App requests RC capabilities for SystemCapabilityType = REMOTE_CONTROL
_Expected:_

2. SDL checks appHMIType if requested SystemCapability is allowed for such app
3. SDL checks if capabilities for requested moduleType is allowed by Policy for such app
4. SDL checks if the requested Interface capability is available on HMI
5. SDL checks if the requested moduleType capability was provided by HMI
6. SDL checks "moduleName" parameter
7. App receives current RC capabilities 
SDL -> app: GetSystemCapability(requested_RemoteControlCapabilites)

**Exceptions:**

2.1 mobile app registerred with different from REMOTE_CONTROL appHMIType, SDL responds - GetSystemCapability (DISALLOWED, success:false)

3.1 moduleType is not assigned in mobile app's Policy, SDL responds - GetSystemCapability (DISALLOWED, success:false)

3.2 application requested capabilities for a few moduleTypes and some of them are not allowed by Policy, SDL responds GetSystemCapability (<only_allowed_by_policy>RemoteControlCapability)

4.1 HMI responded RC.Isready(available:false) - SDL responds GetSystemCapability (UNSUPPORTED_RESOURCE)

5.1 If HMI did not provide capabilities for the signed moduleType -> SDL provides capabilities from HMI_capabilities.json


> Requirement [#1](https://github.com/smartdevicelink/sdl_requirements/issues/1)

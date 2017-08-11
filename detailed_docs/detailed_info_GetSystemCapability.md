## Use Case 1: SDL requests RC capabilities from HMI

**Main Flow:**

_Pre-conditions:_

a. new ignition cycle.  SDL and HMI just started and initializing

_Steps:_

1. SDL sends to HMI requests to start work correctly
2. SDL requests HMI if RC interface is available

_Expected:_

3. HMI responds that RC interface is available: true
4. SDL requests RC.GetCapabilities from HMI
5. HMI responds with RC.GetCapabilities(remoteControlCapabilities)
6. SDL stores received capabilities and uses during ignition cycle

_Exception 1:_

5.1 HMI response is invalid

5.2 SDL is using default capabiltites during ignition cycle stored in HMI_capabilities.json file

**Alternative flow 1:**

3.a.1 HMI responds that RC interface is available: false

3.a.2 Interface is not available, all RPCs from mobile application to such interface will be rejected by SDL with result code UNSUPPORTED_RESOURCE, success:false 

**Alternative flow 2:**

3.b.1 HMI didn't respond on RC.IsReady request from SDL

3.b.2 SDL requests RemoteControlCapabilities from HMI

3.b.3 HMI responds with RC capabilities

3.b.4 SDL checks the received response and response is valid

3.b.5 SDL is using received RC capabilties during ingnition cycle

_Exception 2.1:_

3.b.3.a HMI did not respond on capabilities request

3.b.3.b SDL is using default capabiltites during ignition cycle stored in HMI_capabilities.json file

3.b.3.c SDL is processing RC-related RPCs

_Exception 2.2:_

3.b.4.a Received response from HMI is invalid

3.b.4.b SDL is using default capabiltites during ignition cycle stored in HMI_capabilities.json file

3.b.4.c SDL is processing RC-related RPCs

**Alternative flow 3:**

5.a.1 HMI did not provide RemoteControlCapabilities but RC interface is available:true

5.a.2 SDL checks if the HMI_capabilities.json file is available by the path signed in .ini file

5.a.3 SDL uses default RC capabilities from HMI_capabilities.json file


## Use Case 2: Mobile app requests RC capabilities

**Main Flow:**

_Pre-conditions:_

a. mobile app is registered with REMOTE_CONTROL appHMIType with SDL 

_Steps:_

1. App requests RC capabilities for SystemCapabilityType = REMOTE_CONTROL

_Expected:_

2. SDL checks appHMIType if requested SystemCapability is allowed for such app
3. SDL checks if requested capabilities align with allowed by Policy moduleType for such app
4. SDL checks if requested capabilities are valid
5. SDL checks if the requested Interface capability is available on HMI
6. SDL checks if the requested moduleType capability was provided by HMI
7. App receives current RC capabilities 
>SDL -> app: GetSystemCapability(requested_RemoteControlCapabilites)

**Exceptions:**

2.1 mobile app registerred with different from REMOTE_CONTROL appHMIType, SDL responds - GetSystemCapability (DISALLOWED, success:false)

3.1 requested capability does not align with moduleType assigned in mobile app's Policy, SDL responds - GetSystemCapability (DISALLOWED, success:false)

3.2 application requested capabilities for a few moduleTypes and some of them are not allowed by Policy, SDL responds GetSystemCapability (<only_allowed_by_policy>RemoteControlCapability)

4.1 Mobile application requested RC capabilities with invalid parameters, SDL cuts off invalid parameters and responds with capabilities list for requested valid parameters only

5.1 RC interface is not available on HMI, SDL responds to mobile application GetSystemCapability (UNSUPPORTED_RESOURCE, success:false)

6.1 HMI did not provide capabilities for the signed moduleType, SDL responds UNSUPPORTED_RESOURCE, success:false to mobile RPCs that requested such module


> Requirement [#1](https://github.com/smartdevicelink/sdl_requirements/issues/1)

## Use Case 1: Disabling RC-functionality

**Main Flow:**

_Pre-conditions:_

a. HMI and SDL are started

b. RC interface is available on HMI

c. apps are registered on SDL with REMOTE_CONTROL appHMIType

d. apps are in HMILevel other than NONE and subscribed to the available RC_modules on HMI

_Steps:_

1. User chose to disable remote_control functionality via HMI screen

_Expected:_

2. HMI sends notification to SDL that remote_control functionality is disabled
3. SDL does not change RC applications HMI levels
4. SDL keeps all applications with REMOTE_CONTROL appHMIType registered
5. SDL unsubscribes all applications from RC_modules on HMI
6. SDL releases all allocated RC_modules
7. All RC-requests from applications with REMOTE_CONTROL appHMIType are not processed

## Use Case 2: Enabling RC-functionality

**Main Flow:**

_Pre-conditions:_

a. HMI and SDL are started

b. RC interface is available on HMI

c. User disabled RC functionality on HMI

d. apps are registered on SDL with REMOTE_CONTROL appHMIType

e. RC apps are in HMILevel NONE

_Steps:_

1. User chose to enable remote_control functionality via HMI screen

_Expected:_

2. HMI sends notification to SDL that remote_control functionality is enabled

> Requirement: [#11](https://github.com/smartdevicelink/sdl_requirements/issues/11) [SDL_RC] Disable/enable RC-feature

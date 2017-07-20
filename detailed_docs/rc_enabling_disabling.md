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
3. SDL assigns HMIlevel NONE to all applications with REMOTE_CONTROL appHMIType
4. SDL sends OnHMIStatus (NONE) to all applications with REMOTE_CONTROL appHMIType
5. SDL keeps all applications with REMOTE_CONTROL appHMIType registered
6. SDL unsubscribes all applications from RC_modules on HMI
7. HMILevel cannot be changed to other than NONE until RC functionality is enabled
8. All RC-requests from applications with REMOTE_CONTROL appHMIType are not processed

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

3. All RC applications remain in appHMILevel NONE until activated by user

> Requirement: [#11](https://github.com/smartdevicelink/sdl_requirements/issues/11) [SDL_RC] Disable/enable RC-feature

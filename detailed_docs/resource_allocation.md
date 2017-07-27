## Use Case 1: Resource allocation 

**Main Flow:**

_Pre-conditions:_

a. HMI and SDL are started, RC interface is available on HMI

b. RC_app_1 and RC_app_2 are registered on SDL with "REMOTE_CONTROL" appHMIType

c. module_1 is in "free" state on HMI

_Steps:_

1. User chose to allow RC functionality via HMI and specified access mode as AUTO_ALLOW or didn't specify the access mode at all
2. HMI sends notification to SDL that Remote control functionality is allowed with or without specified access mode
3. RC_app_1 is in HMILevel FULL and is in control of the module_1 (sent request to either change settings or emulate button press)
4. User activates RC_app_2 (app_2 is in FULL HMI level now)
5. RC_app_2 sends control request to module_1 (sent request to either change settings or emulate button press)

_Expected:_

6. RC_app_2 gets in control of the module_1

_Exception 1:_

4.1 User did not activate RC_app_2 (app_2 is in LIMITTED or BACKGROUND HMILevel)

4.2 RC_app_2 sends control request to module_1 (sent request to either change settings or emulate button press)

4.3 RC_app_2 control request was rejected with result code REJECTED, success:false, RC_app_1 remains in control of module_1 

_Exception 2:_

3.1 RC_app_1  is in control of the module_1 (sent request to either change settings or emulate button press)

3.2 User activates RC_app_2 (app_2 is in FULL HMI level now)

3.3 module_1 is currently executing one of the RC requests of RC_app_1

3.4 RC_app_2 sends control request to module_1 (sent request to either change settings or emulate button press)

3.5 RC_app_2 control request was rejected with result code IN_USE, RC_app_1 remains in control of module_1

**Alternative flow 1**

1.a.1 User chose allow RC functionality via HMI and specified access mode as AUTO_DENY

1.a.2 HMI sends notification to SDL that Remote control functionality is allowed with specified access mode

1.a.3 RC_app_1 is in HMILevel FULL and is in control of the module_1 (sent request to either change settings or emulate button press)

1.a.4 User activates RC_app_2 (app_2 is in FULL HMI level now)

1.a.5 RC_app_2 sends control request to module_1 (sent request to either change settings or emulate button press)

_Expected:_

1.a.6 RC_app_2 control request was rejected with result code IN_USE, RC_app_1 remains in control of module_1

**Alternative flow 2**

1.b.1 User chose allow RC functionality via HMI and specified access mode as ASK_DRIVER

1.b.2 HMI sends notification to SDL that Remote control functionality is allowed with specified access mode

1.b.3 RC_app_1 is in HMILevel FULL and is in control of the module_1 (sent request to either change settings or emulate button press)

1.b.4 User activates RC_app_2 (app_2 is in FULL HMI level now)

1.b.5 RC_app_2 sends control request to module_1 (sent request to either change settings or emulate button press)

_Expected:_

1.b.6 SDL sends request to HMI to display permission prompt to the driver

1.b.7 Pop-up on the screen of head unit asks driver to allow RC_app_2 access to module_1 

1.b.8 Driver allows access for RC_app_2 to get in control of module_1

1.b.9 HMI responds to SDL with user consent (allowed: true) reagrding RC_app_2 access to module_1

1.b.10 RC_app_2 gets in control of the module_1

_Exception:_

1.b.9.1 HMI does not respond during default time out or responds with invalid parameters

1.b.9.2 SDL responds with result code GENERIC_ERROR to RC_app_2 control request, RC_app_1 remains in control of module_1. 
New control requests from RC_app_2 to module_1 if it is still in use by another application shall trigger new driver consent requests.

**Alternative flow 2.1**

1.b.8.a.1 Driver denies access for RC_app_2 to get in control of module_1

_Expected:_

1.b.8.a.2  HMI responds to SDL with user consent (allowed:false) ragrding RC_app_2 access to module_1

1.b.8.a.3 RC_app_2 control request is rejected with result code REJECTED, success:false,  info: "The resource is in use and the driver disallows this remote control RPC", RC_app_1 remains in control of module_1

1.b.8.a.4 RC_app_2 requests control again for the same module_1

1.b.8.a.5 SDL rejects the request without new driver's permission request with result code REJECTED, success:false

**Alternative flow 2.2**

1.b.8.b.1 Driver did not respond on consent prompt during default timeout

_Expected:_

1.b.8.b.2 HMI responds to SDL with result code TIMED_OUT

1.b.8.b.3 RC_app_2 control request is rejected with result code TIMED_OUT, success: false, info: "The resource is in use and the driver did not respond in time", RC_app_1 remains in control of module_1

1.b.8.b.4 RC_app_2 requests control again for the same module_1

1.b.8.b.5 SDL sends new request to HMI to display permission prompt to the driver


> Requirement: [#10](https://github.com/smartdevicelink/sdl_requirements/issues/10) [SDL_RC] Resource allocation based on access mode

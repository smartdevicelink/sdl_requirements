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

## Use Case 2: Media app sets audio source to itself (or other system sources)

**Main Flow:**

_Pre-conditions:_  
a. mobile device is connected to SDL  

b. media app is registered with REMOTE_CONTROL appHMIType with SDL from this device

c. app-assigned policies allow SetInteriorVehicleData RPC

d. moduleType_AUDIO is allowed for this app by policies

e. app is running in FULL or LIMITED HMI level

f. app's Audio Source=MOBILE_APP (keepContext = true)

_Steps:_  

1. App requests to change audio source to either itself (with target MOBILE_APP) or other system sources

_Expected:_  

2. SDL transfers request to HMI
3. SDL receives the response from HMI
4. SDL transfers received response from HMI to the app
5. SDL does not change app's HMI level
6. Requested settings are successfully changed on HMI according to application's request  

**Alternative flow 1**  
1.a.1 app is running in BACKGROUND requests to switch the audio source from others to itself  

_Expected:_
1.a.2 application sends an alert with at least "yes" and "no" soft buttons  
1.a.3 SDL transfers the request to HMI displayed on the Alert dialog
1.a.4 User presses Alert "yes" softButton 
1.a.5 HMI brings the application to foreground 
1.a.6 SDL changes app's HMI level into FULL
1.a.7 HMI sets audio source to this media application  
1.a.8 SDL transfers received response from HMI to the app  
1.a.9 Requested settings are successfully changed on HMI according to application's request
 

## Use Case 3: Media app sets audio source from itself (to other types of audio source)  

**Main Flow:**

_Pre-conditions:_

a. mobile device is connected to SDL  

b. media app is registered with REMOTE_CONTROL appHMIType with SDL from this device

c. app-assigned policies allow SetInteriorVehicleData RPC

d. moduleType_AUDIO is allowed for this app by policies

e. app is running in FULL or LIMITED HMI level

f. current Audio Source=MOBILE_APP (keepContext = false) or keepContext is omitted  

_Steps:_  

1.	App requests to change audio source from MOBILE_APP to other types of audio source

_Expected:_  
2. SDL transfers request to HMI
3. SDL receives the response from HMI
4. SDL transfers received response from HMI to the app
5. SDL changes app’s HMI level into BACKGROUND
6. Requested settings are successfully changed on HMI according to application's request  

**Exception:**  
5.1 SDL does not change app’s HMI level in case keepContext = true 

**Alternative flow 1**  
1.a.1 App requests to change audio source from any type except for MOBILE_APP (with any value of keepContext parameter or without it)  

_Expected:_  
1.a.2. SDL transfers request to HMI  
1.a.3. SDL receives the response from HMI  
1.a.4. SDL transfers received response from HMI to the app  
1.a.5. SDL does not change app’s HMI level  
1.a.6. Requested settings are successfully changed on HMI according to application's request


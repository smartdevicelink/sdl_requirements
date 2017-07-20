## Use Case 1: Unsubscribe from RC module status change notification

**Main Flow:**

_Pre-conditions:_

a. mobile device is connected to SDL.

b. app is registered with REMOTE_CONTROL appHMIType with SDL from this device. 

c. app issubscribed on RC module status change notification

_Steps:_

1. App requests to unsubscribe from RC module status change notification

_Expected:_

2. SDL transfers request to HMI
3. SDL receives the response from HMI
4. SDL unsubscribes the app from RC module status change notification
5. SDL transfers received response from HMI to the app
6. Emulate any changes in module data either by changing settings via button press on HMI or commands from application
7. HMI sends notification about changes in module data to SDL
8. Application does not receive notification with module data changes 

**Alternative flow 1**

3.a.1 In response from HMI, SDL received "isSubscribed:true"

_Expected:_

3.a.2 SDL does not unsubscribe the app from RC module status change notification

3.a.3 SDL transfers received response from HMI to the app

3.a.4 After changing settings in moduleData on HMI, HMI sends notification about changes to SDL

3.a.5 SDL transfers the notification to the app

**Alternative flow 2**

3.b.1 In response from HMI, SDL did not receive "isSubscribed" parameter

_Expected:_

3.b.2 SDL does not unsubscribe the app from RC module status change notification

3.b.3 SDL transfers received response from HMI to the app

3.b.4 After changing settings in moduleData on HMI, HMI sends notification about changes to SDL

3.b.5 SDL transfers the notification to the app

**Alternative flow 3**

3.c.1 SDL received response from HMI with any_erroneous_result

_Expected:_

3.c.2 SDL does not unsubscribe the app from RC module status change notification

3.c.3 SDL transfers received response from HMI to the app

3.c.4 After changing settings in moduleData on HMI, HMI sent notification about changes to SDL

3.c.5 SDL transfers the notification to the app


> [#5](https://github.com/smartdevicelink/sdl_requirements/issues/5)[SDL_RC] Unsubscribe from RC module change notifications

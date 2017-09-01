## Use Case 1: Subscription on module status change notification

**Main Flow:**

_Pre-conditions:_

a. mobile device is connected to SDL.

b. app is registered with REMOTE_CONTROL appHMIType with SDL from this device. 

c. app is not subscribed on RC module status change notification

_Steps:_

1. App sends request to subscribe on RC module status change notification

_Expected:_

2. SDL transfers request to HMI
3. SDL receives the response from HMI with "isSubscribed:true"
4. SDL subscribes the app on RC module status change notification based on response from HMI
5. SDL transfers received response from HMI to the app
6. Emulate any changes in module data either by changing settings via button press on HMI or commands from application
7. HMI sends notification about changes in module data to SDL
7. Application receives notification with module data changes in settings

**Alternative flow 1**

3.a.1 In response from HMI, SDL received "isSubscribed:false"

_Expected:_

3.a.2 SDL does not subscribe the app on RC module status change notification

3.a.3 SDL transfers received response from HMI to the application

3.a.4 Emulate any changes in module data either by changing settings via button press on HMI or commands from application

3.a.5 HMI sends notification about changes in module data to SDL

3.a.6 SDL does not transfer the notification to the application

**Alternative flow 2**

3.b.1 In response from HMI, SDL did not receive "isSubscribed" parameter

_Expected:_

3.b.2 SDL does not subscribe the application on RC module status change notification

3.b.3 SDL transfers received response from HMI to the application

3.b.4 Emulate any changes in module data either by changing settings via button press on HMI or commands from application

3.b.5 HMI sends notification about changes in module data to SDL

3.b.6 SDL does not transfer the notification to the app

**Alternative flow 3**

3.c.1 SDL received response from HMI with any_erroneous_result

_Expected:_

3.c.2 SDL does not subscribe the app on RC module status change notification

3.c.3 SDL transfers received response from HMI to the app

3.c.4 After changing settings in moduleData on HMI, HMI sent notification about changes to SDL

3.c.5 SDL does not transfer the notification to the app

> [#4](https://github.com/smartdevicelink/sdl_requirements/issues/4) [SDL_RC] Subscrie on RC module change notification

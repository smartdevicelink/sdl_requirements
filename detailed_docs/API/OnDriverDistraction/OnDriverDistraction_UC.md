## Use Case 1: Notification about Driver Distraction mode activation to already registered apps

**Main Flow:**

_Pre-conditions:_

a.	SDL and HMI are started  
b.	Mobile applications are registered and running on SDL

_Steps:_

1.	HMI sends OnDriverDistraction notification to SDL

_Expected:_  
2.	SDL checks if OnDriverDistraction notification is valid  
3.	SDL checks if the app in current HMILevel is allowed to receive OnDriverDistraction notification (defined by app's assigned Policies)  
4.	SDL transfers OnDriverDistraction notification to all applications being connected to SDL at the moment on any device

**Exceptions 1:**  
2.1 Received notification from HMI is invalid  
2.2 SDL logs the error internally and ignores such notification without transferring it registered applications

**Exceptions 2:**  
3.2 App is in NONE HMI level and is not allowed to receive OnDriverDistraction notification  
3.3 SDL doesn't transfer the notification to such mobile application

## Use Case 2: Notification about Driver Distraction mode activation to newly registered app 

_Pre-conditions:_

a.	SDL and HMI are started  
b.	Mobile app registers and gets NONE HMILevel  
c.  Mob app in NONE HMILevel is NOT allowed to get OnDriverDistraction notification by Policies

_Steps:_
1. HMI sends OnDriverDistraction notification to SDL

_Expected:_  
2.	SDL checks if OnDriverDistraction notification is valid  
3.	SDL internally stores the values for `<state>` 
4.	SDL transfers OnDriverDistraction notification with the last known (actual) state to this mobile app right after the app changes HMILevel to any other than NONE

**Alternative flow:**  
c.1 Mob app in NONE HMILevel is allowed to get OnDriverDistraction notification by Policies  
c.2 HMI sends OnDriverDistraction notification to SDL  
c.3 SDL checks if OnDriverDistraction notification is valid  
c.4 SDL transfers valid OnDriverDistraction notification to mob app


## Use Case 3: Multiple apps - Sending Driver Distraction status when 1 app state changes from NONE

_Pre-conditions:_  

a.	SDL and HMI are started  
b.	app_1 is registered on SDL and has FULL HMILevel  
c.	app_2 is registered on SDL and has BACKGROUND HMILevel  
d.	app_3 is registered and on SDL and has NONE HMILevel  
e.  Mob apps in NONE HMILevel are NOT allowed to get OnDriverDistraction notification by Policies

_Steps:_ 

1. HMI sends OnDriverDistraction notification to SDL
2. SDL checks if OnDriverDistraction notification is valid
3. SDL transfers OnDriverDistraction notification to app_1, app_2 (HMI Levels are allowed by Policies)  
4. User activates app_3 being in NONE 

_Expected:_

5. HMI sends request to activate app_3 to SDL
6. SDL sends OnHMIStatus (FULL) to app_3
7. SDL sends OnDriverDistraction notification only to app_3

**Alternative flow:**  
e.1 Mob app in NONE HMILevel is allowed to get OnDriverDistraction notification by Policies  
e.2 HMI sends OnDriverDistraction notification to SDL  
e.3 SDL checks if OnDriverDistraction notification is valid  
e.4 SDL transfers valid OnDriverDistraction notification to mob app_1, app_2, app_3  
e.5 SDL does not send OnDriverDistraction notification to app_3 when app_3 changes its HMI Level from NONE to any other

## Use Case 4: Transferring notification after mobile app registration

_Pre-conditions:_

a.	SDL and HMI are started

_Steps:_
1. Mobile app successfully connects and gets any HMILevel allowed by Policies to receive OnDriverDistraction notification

_Expected:_ 

2. SDL sends OnDriverDistraction notofication with current state to this connected app

**Note**: By default OnDriverDistraction(state=OFF) 

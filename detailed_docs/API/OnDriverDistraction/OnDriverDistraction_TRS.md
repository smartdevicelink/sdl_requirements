## Functional requirements

### General info
Driver distraction rules (if active) are expected to be configurable on HMI for each platform.  
HMI must inform SDL via OnDriverDistraction whenever the driver distraction mode is activated/deactivated on HMI.  
*Important Note: DD rules are not applicable to SDL functionality, but describe platform (head unit) configuration and driver distraction restrictions to display or operate some functionality on HMI.*

Driver distraction rules may be specific to country/area, so it depends on HMI when to trigger activate/deactivate states.
For current HMI configuration, sub-menu opening and ScrollableMessage operating are not available during DD ON mode. HMI may notify user about DD mode enabling-disabling.

## Functional requirements

### Transferring notification received from HMI

1.  
In case
SDL received valid OnDriverDistraction notification from HMI
and the app is allowed to receive the notification in its current HMI Level

SDL must  
transfer OnDriverDistraction notification with all mandatory fields to all applications being registered to SDL

_Information: Whether the app receives the notification in current HMILevel is defined by app's assigned Policies._

2.  
In case  
Policy Table doesn't contain current application's HMILevel defined in Policy Table "functional_groupings" section for a specified notification  

SDL must  
NOT send OR transfer (in case got from HMI) notification to mobile application

3.  
In case  
the app is registered with SDL, running in NONE HMI Level  
SDL received valid OnDriverDistraction(`<state>`) notification from HMI  
and apps in NONE HMI Level are not allowed to receive OnDriverDistraction notifications by Policies

SDL must  
transfer the last known (actual) OnDriverDistraction (`<state>`) and to this mobile app right after this mobile app changes HMILevel to any other than NONE

4.  
In case
SDL received invalid OnDriverDistraction notification from HMI 

SDL must  
log the issue and ignore this notification

_Information:_  
Invalid notifications means the notification contains:  
a. params out of bounds or incorrect symbols (/t, /n, `<whitespace`>)  
b. mandatory params are missing  
c. params of wrong type  
d. invalid json  
e. incorrect combination of params

### Transferring OnDriverDistraction notification depending on `lock_screen_dismissal_enabled` value in Policy DB

5.
In case  
Policy DB has "lock_screen_dismissal_enabled=true"  
Policy allows a mobile app to receive OnDriverDistraction notification in current HMI level  
HMI sends a valid OnDriverDistraction(state=DD_ON) notification to SDL

SDL must  
transfer OnDriverDistraction notification with `state=DD_ON` and `lockScreenDismissalEnabled=true` to the mobile app

6.  
In case  
Policy DB has "lock_screen_dismissal_enabled=false"  
Policy allows a mobile app to receive OnDriverDistraction notification in current HMI level  
and HMI sends a valid OnDriverDistraction notification with state=DD_ON to SDL

SDL must  
transfer OnDriverDistraction notification with state=DD_ON and lockScreenDismissalEnabled=false to the mobile app

7.  
In case  
Policy DB does not have "lock_screen_dismissal_enabled" defined  
Policy allows a mobile app to receive OnDriverDistraction notification in current HMI level  
HMI sends a valid OnDriverDistraction notification to SDL

SDL must  
transfer OnDriverDistraction notification without `lockScreenDismissalEnabled` to the mobile app

8.  
In case  
Policy DB has "lock_screen_dismissal_enabled=true"  
Policy allows a mobile app to receive OnDriverDistraction notification in current HMI level  
HMI sends a valid OnDriverDistraction(state=DD_OFF) notification to SDL

SDL must  
transfer OnDriverDistraction notification without `lockScreenDismissalEnabled` to the mobile app

#### Transferring OnDriverDistraction notification after `lock_screen_dismissal_enabled` value was updated after PTU

9.  
In case  
the app is in NONE  
and the app is allowed to receive the notification in all HMI Levels by Policies  
and Initially policy DB doesn't have "lock_screen_dismissal_enabled" defined  
HMI sends OnDriverDistraction(DD=ON)

SDL must  
transfer OnDriverDistraction(DD=ON) without `lockScreenDismissalEnabled` to the mobile app

and PTU occurs  
After PTU "lock_screen_dismissal_enabled" value was configured as "true/false"   

SDL must  
send OnDriverDistraction(DD=ON) with configured `lockScreenDismissalEnabled` "true/false" value


10.
In case  
the app is in NONE  
and the app is allowed to receive the notification in all HMI Levels by Policies  
and Initially policy DB has "lock_screen_dismissal_enabled" configured with "true/false" value  
HMI sends OnDriverDistraction(DD=ON)

SDL must  
transfer OnDriverDistraction(DD=ON) with `lockScreenDismissalEnabled` "true/false" value to the mobile app

and PTU occurs  
After PTU "lock_screen_dismissal_enabled" value was missed

SDL must  
send OnDriverDistraction(DD=ON) without `lockScreenDismissalEnabled` value

11.
In case  
the app is in NONE  
and the app is allowed to receive the notification in all HMI Levels by Policies  
and Initially policy DB doesn't have "lock_screen_dismissal_enabled" defined  
HMI sends OnDriverDistraction(DD=OFF)

SDL must  
transfer OnDriverDistraction(DD=OFF) without `lockScreenDismissalEnabled` to the mobile app

and PTU occurs  
After PTU "lock_screen_dismissal_enabled" value was configured as "true/false" 

SDL must  
NOT transfer OnDriverDistraction(DD=OFF) after updating `lockScreenDismissalEnabled` value due to PTU

## Non-functional requirements
1. New `lockScreenDismissalEnabled` parameter must be added to Mobile_API

```
    <function name="OnDriverDistraction" functionID="OnDriverDistractionID" messagetype="notification" since="1.0">
        <description>Provides driver distraction state to mobile applications</description>
        <param name="state" type="DriverDistractionState" mandatory="true">
            <description>Current State of Driver Distraction</description>
        </param>
    </function>
    <!-- newly added parameter -->
    <param name="lockScreenDismissalEnabled" type="Boolean" mandatory="false">
    <description>
      If enabled, the lock screen will be able to be dismissed while connected to SDL, allowing users 
      the ability to interact with the app. Dismissals should include a warning to the user and ensure 
      that they are not the driver.
    </description>
  </param>
 ```  
 
2. HMI_API
```
  <function name="OnDriverDistraction" messagetype="notification">
    <description>Notification must be sent from HMI to SDL when driver distraction state is changed. Driver distraction rules are defined by the platform.</description>
    <param name="state" type="Common.DriverDistractionState" mandatory="true">
      <description>See DriverDistractionState. </description>
    </param>
  </function>
``` 
3. PolicyTable must support new `lock_screen_dismissal_enabled` parameter set by default as 'true' to module_config section of PT.
```
{
    "policy_table": {
        "module_config": {
            "preloaded_pt": true,
            "exchange_after_x_ignition_cycles": 100,
            "exchange_after_x_kilometers": 1800,
            "exchange_after_x_days": 30,
            "timeout_after_x_seconds": 60,
            "seconds_between_retries": [1,
            5,
            25,
            125,
            625],
            "lock_screen_dismissal_enabled": true
  ```

## Diagram
OnDriverDistraction
![OnDriverDistraction](./accessories/OnDriverDistraction_lockScreenDismissalEnabled.png)

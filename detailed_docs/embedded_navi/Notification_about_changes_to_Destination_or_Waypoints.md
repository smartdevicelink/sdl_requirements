## Use Case 1:Notification about changes to Destination or Waypoints

**Main Flow:**

_Pre-conditions:_

a. SDL and HMI are started

b. Navi interface and embedded navigation source are available on HMI

c. Mobile applications are registered on SDL

d. Mobile applications are subscribed on destination and waypoints changes notifications

_Steps:_

1. Any change in destination or waypoints is registered on HMI (user set new route, canselled the route, arrived at destination point or crossed a waypoint)

2. HMI sends the notification about changes to SDL

_Expected:_

3. SDL transfers the notification about changes to destination or waypoints to mobile application

**Exception 1**

2.1.a Notification about changes to destination or waypoints is not allowed by Policies for mobile application

2.1.b SDL logs corresponding error internally, ignores received notification from HMI and doesn't transfer it to mobile application

**exception 2**

2.2.a Received notification about changes to destination or waypoints from HMI is invalid

2.2.b SDL logs the error internally and ignores such notification without transfering it to any of subscribed mobile applications

**Alternative flow 1**

1.a.1 One application requested to unsubscribe from receiving notifications on destination & waypoints changes (other mobile applications remain subscribed)

1.a.2 Any change in destination or waypoints is registered on HMI (user set new route, canselled the route, arrived at destination point or crossed a waypoint)

1.a.3 HMI sends the notification about changes to SDL

_Expected:_

1.a.4 SDL does not transfer the notification to unsubscribed mobile application

1.a.5 SDL transfers the notification to subscribed mobile applications

**Alterbnative flow 1.1**

1.a.1.1 Application requested to unsubscribe from receiving notifications on destination & waypoints changes and no other subscribed applications left

1.a.1.2 Any change in destination or waypoints is registered on HMI (user set new route, canselled the route, arrived at destination point or crossed a waypoint)

_Expected:_

1.a.1.3 HMI doesn't send any notifications about changes to SDL

1.a.1.4 SDL doesn't send any notifications about changes to any of the registered applications

> _Requirement [28](https://github.com/smartdevicelink/sdl_requirements/issues/28) Notification about changes to Destination or Waypoints_OnWayPointChange_

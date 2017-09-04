## Use Case 1:Notification about changes to Destination or Waypoints

**Main Flow:**

_Pre-conditions:_

a. SDL and HMI are started

b. Navi interface and embedded navigation source are available on HMI

c. Mobile application is registered on SDL

d. Mobile application is subscribed on destination and waypoints changes notifications

_Steps:_

1. Any change in destination or waypoints is registered on HMI (user set new route, canselled the route, arrived at destination point or crossed a waypoint)

2. HMI sends the notification about changes to SDL

_Expected:_

3. SDL transfers the notification about changes to destination or waypoints to mobile application

**Alternative flow 1**

1.a.1 Application requested to unsubscribe from receiving notifications on destination & waypoints changes

1.a.2 Any change in destination or waypoints is registered on HMI (user set new route, canselled the route, arrived at destination point or crossed a waypoint)

1.a.3 HMI sends the notification about changes to SDL

_Expected:_

1.a.4 SDL does not transfer the notification to mobile application

> _Requirement [28](https://github.com/smartdevicelink/sdl_requirements/issues/28) Notification about changes to Destination or Waypoints_OnWayPointChange_

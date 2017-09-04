## Use Case 1: 	Unsubscribe from Destination & Waypoints

**Main Flow:**

_Pre-conditions:_

a. SDL and HMI are started

b. Mobile application is registered on SDL

c. Mobile application is subscribed on destination & Waypoints

_Steps:_

1. Mobile application requests to unsubscribe from destination and waypoints changes notifications
2. SDL validates parameters of the request
3. SDL checks if the request is allowed by Policies
4. SDL checks if Navigation interface is available on HMI
5. SDL transfers the request with valid and allowed parameters to HMI
6. SDL receives response from HMI
7. SDL stores the subscription status of the application internally
8. SDL transfers response to mobile app
9. Any change on destination or waypoints happens on HMI

_Expected:_

10. HMI sends the notification to SDL with the changes in waypoints or destination
11. SDL does not transfer the notification to unsubscribed application

**Alternative flow 1**

2.a.1 Request is invalid

2.a.2 SDL responds INVALID_DATA, success:false to mobile application and doesn't unsubscribe from destination and waypoints change notifications

**Alternative flow 2:**

3.a.1 Request is not allowed by Policies

3.a.2 SDL responds DISALLOWED, success:false to mobile app and doesn't unsubscribe from destination and waypoints change notifications

**Alternative flow 3:**

4.a.1 Navigation interface is not available on HMI

4.a.2 SDL responds UNSUPPORTED_RESOURCE, success:false to mobile app and doesn't unsubscribe from destination and waypoints change notifications

**Exception 1:**

6.1.a HMI did not respond during default timeout

6.1.b SDL responds GENERIC_ERROR, success:false to mobile application and does not unsubscribe from destination and waypoints change notifications

**Exception 2:**

6.2.a HMI responds with UNSUPPORTED_RESOURCE

6.2.b SDL transfer UNSUPPORTED_RESOURCE, success:false to mobile application and does not unsubscribe from destination and waypoints change notifications

## Use Case 2: Mobile application is already unsubscribed from Destination & Waypoints

**Main Flow:**

_Pre-conditions:_

a. SDL amd HMI are started

b. Mobile application is registered on SDL

c. Mobile application is unsubscribed from destination & waypoints change notification

_Steps:_

1. The same mobile application sends new request to unsubscribe from destination and waypoints change notification

_Expected:_

2. SDL responds with result code IGNORED, success:false to mobile application
3. SDL keeps the subscription status of the application unchanged (application remains unsubscribed)

**Alternative flow 1:**

1.a.1 Mobile application unexpectedly disconnects

_Expected:_

1.a.2 SDL stores the subscription status internally (application remains unsubscribed)

1.a.3 SDL restores the subscription status right after application reconnects with the same hashID that was before disconnect

**Alternative flow 2:**

1.b.1 Ignition OFF - ignition cycle is over

1.b.2 New ignitions cycle, mobile application registers with the same hashID that was in previous ignition cycle

_Expected:_

1.b.3 SDL restores the subscription status of mobile application (application remains unsubscribed)

> _Requirement [27](https://github.com/smartdevicelink/sdl_requirements/issues/27) Unsubscribe from Destination & Waypoints_UnsubscribeWayPoints_

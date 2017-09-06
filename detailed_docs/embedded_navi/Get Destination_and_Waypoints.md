## Use Case 1: Get Destination & Waypoints

**Main Flow:**

_Pre-conditions:_

a. SDL and HMI are started

b. Mobile application is registered on SDL

_Steps:_

1. appID requests to get details of the destination and waypoints set on the system so that it can provide last mile connectivity.

_Expected:_

2. SDL validates parameters of the request
3. SDL checks if the request is allowed by Policies
4. SDL checks if Navi interface is available on HMI
5. SDL checks if  "getWayPointsEnabled" is enabled in HMI capabilities
6. SDL transfers the request with valid and allowed parameters to HMI
7. SDL receives response from HMI
8. SDL transfers response to mobile app

**Exception 1:**

2.1.a Request is invalid

2.1.b SDL responds INVALID_DATA, success:false to mobile app and doesn't transfer request to HMI

**Exception 2:**

3.1.a Request is not allowed by Policies

3.1.b SDL responds DISALLOWED, success:false to mobile app and doesn't transfer this request to HMI

**Exception 3:**

4.1.a Navigation interface is not available on HMI

4.1.b SDL responds UNSUPPORTED_RESOURCE, success:false ot mobile app and doesn't transfer this request to HMI

**Exception 4:**

5.1.a "getWayPointsEnabled": false in HMI cqapabilities

5.1.b SDL responds UNSUPPORTED_RESOURCE, success:false ot mobile app and doesn't transfer this request to HMI

**Exception 5:**

7.1.a HMI did not respond during default timeout

7.1.b SDL responds GENERIC_ERROR, success:false to mobile app

**Exception 6:**

7.2.a Response from HMI is invalid

7.2.b SDL responds GENERIC_ERROR, success:false to mobile app

> _Requirement [25](https://github.com/smartdevicelink/sdl_requirements/issues/25) Get Destination & Waypoints_GetWayPoints_

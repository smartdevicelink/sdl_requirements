## Functional Requirements

1.
In case mobile application sent valid and allowed by Policies SubscribeWayPoints_request to SDL

SDL must: 
- transfer SubscribeWayPoints_request_ to HMI
- respond with <'resultCode'> received from HMI to mobile app 

2.
In case mobile application sent valid SubscribeWayPoints_request to SDL

and this request is NOT allowed by Policies

SDL must: 

respond "DISALLOWED, success:false" to mobile app

3.
In case mobile application sent valid and allowed by Policies SubscribeWayPoints_request to SDL

and Navigation interface ios not available on HMI

SDL must:

respond UNSUPPORTED_RESOURCE, success:false to mobile application without transferring this request to HMI

4.
In case mobile application is already subscribed on WayPoints-related parameters

and the same mobile application sends SubscribeWayPoints_request to SDL

SDL must:

- respond "IGNORED, success:false" to mobile application
- keep application subscribed on waypoints change notifications

5.
In case mobile application is subscribed on WayPoints-related parameters unexpectedly disconnects 

SDL must:
- store the status of subscription on wayPoints-related data for this application
- send UnsubscribeWayPoints_request to HMI ONLY if ther are no other applications currently subscribed to WayPoints-related data 
- restore status of subscription on WayPoints-related data for this application right after the same mobile application re-connects within the same ignition cycle with the same <'hashID'> as before unexpected disconnect
- after successful resumption send SubscribeWayPoints request to HMI only

6.
In case mobile application subscribed on WayPoints-related data in previous ignition cycle
registers during next ignition cycle with the same <'hashID'> as in previous ignition cycle

SDL must:

restore status of subscription on WayPoints-related data being in previous ignition cycle for this application

7.
In case mobile application is already subscribed on WayPoints-related data

and another application sends SubscribeWayPoints_request to SDL

SDL must:
- remember this another app as subscribed on WayPoints-related data
- respond SubscribeWayPoints (SUCCESS) to this other application without transferring secong SubscribeWayPoints_request to HMI 

## Non-Functional Requirements

a. "SubscribeWayPoints" RPC must be added to "functional_groupings" of PolicyTable:

```json
"functional_groupings": {
        "rpcs": {
  "SubscribeWayPoints": {
            "hmi_levels": [
              "BACKGROUND",
              "FULL",
              "LIMITED"
            ]
          },
```

## Diagrams

SubscribeWayPoints

![SubscribeWayPoints](https://github.com/smartdevicelink/sdl_requirements/blob/SubscribeWayPoints/detailed_docs/accessories/SubscribeWayPoints.png)

OnWayPointChnage for newly subscribed application

![OnWayPointChange_storing](https://github.com/smartdevicelink/sdl_requirements/blob/SubscribeWayPoints/detailed_docs/accessories/Store_OnWayPointsChange.png)

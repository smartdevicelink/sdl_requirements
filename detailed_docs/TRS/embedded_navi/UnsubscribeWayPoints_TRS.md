## Functional Requirements

1.
In case mobile application sent valid UnsubscribeWayPoints_request to SDL
- and this request is allowed by Policies
- and there are no other applications subscribed on waypoints change npotifications

SDL must: 
- transfer UnsubscribeWayPoints_request to HMI
- respond with <"resultCode"> received from HMI to mobile application

2.
In case mobile application sent valid UnsubscribeWayPoints_request to SDL
- and this request is allowed by Policies
- and there are other applications still subscribed on waypoints change npotifications

SDL must: 
- unsubscribe requesting application from waypoints change notifications
- not send UnsubscribeWayPoints_reques to HMI

3.
In case mobile application sent valid UnsubscribeWayPoints_request to SDL
and this request is NOT allowed by Policies

SDL must: 

respond "DISALLOWED, success:false" to mobile app

4. 
In case mobile application sent valid and allowed by Policies UnsubscribeWayPoints_request to SDL

and Navigation interface is not available on HMI

SDL must:

respond UNSUPPORTED_RESOURCE, success:false to mobile application without transferring this request to HMI

5.
In case mobile application is already unsubscribed from WayPoints-related parameters

and the same mobile application  sends UnsubscribeVehicleData_request to SDL

SDL must:

- respond "IGNORED, success:false" to mobile application
- keep this application unsubscribed from waypoints change notification

6.
In case mobile application unsubscribed from wayPoints-related parameters
unexpectedly disconnects and re-connects within the same ignition cycle with the same hashID

SDL must:

restore status of unsubscription from wayPoints-related parameters being before unexpected disconnect for this application

7.
In case mobile application was unsubscribed from WayPoints-related parameters in previous ignition cycle
registers during the next ignition cycle with the same hashID

SDL must:

restore status of unsubscription from WayPoints-related parameters as in previous ignition cycle for this application

## Non-Functiona Requirements

a. "UnsubscribeWayPoints" RPC must be added to "functional_groupings" of PolicyTable:

```json
"functional_groupings": {
        "rpcs": {
  "UnsubscribeWayPoints": {
            "hmi_levels": [
              "BACKGROUND",
              "FULL",
              "LIMITED"
            ]
          },
```

## Diagram

UnsubscribeWayPoints

![UnsubscribeWayPoints](https://github.com/smartdevicelink/sdl_requirements/blob/UnsubscribeWayPoints/detailed_docs/accessories/UnsubscribeWayPoints.png)

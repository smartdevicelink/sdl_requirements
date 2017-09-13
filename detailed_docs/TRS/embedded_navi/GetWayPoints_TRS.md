## Functional Requirements

1.
In case mobile application sends valid and allowed by Policies GetWayPoints_request to SDL

SDL must: 
- transfer GetWayPoints_request to HMI
- respond with `<resultCode>` received from HMI to mobile application
- provide the requested parameters at the same order as received from HMI to mobile application (in case of successfull response)

2.
In case mobile application sends valid GetWayPoints_request to SDL

and this request is NOT allowed by Policies

SDL must: 

respond "DISALLOWED, success:false" to mobile application

3. 
In case mobile application sends valid and allowed by Policies GetWayPoints_request to SDL

and Navigation interface is not available on HMI

or

 "getWayPointsEnabled": false in HMI capabilities
 
 SDL must:
 
 respond "UNSUPPORTED_RESOURCE, success:false" to mobile application and not transfer this request to HMI


## Non-Functional Requirements

a. New "GetWayPoits" RPC must be added to "functional_groupings" at PolicyTable:

```json
"functional_groupings": {
        "rpcs": {
  "GetWayPoints": {
            "hmi_levels": [
              "BACKGROUND",
              "FULL",
              "LIMITED"
            ]
          },
```

## Diagram

GetWayPoints

![GetwayPoints](https://github.com/smartdevicelink/sdl_requirements/blob/GetWayPoints/detailed_docs/accessories/GetWayPoints.png)

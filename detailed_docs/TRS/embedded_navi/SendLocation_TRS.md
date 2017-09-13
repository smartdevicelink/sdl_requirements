## Functional Requirements

1.
In case mobile application sends SendLocation_request to SDL 
- **with both** "longitudeDegrees" and "latitudeDegrees" parameters
- **with** "address" parameter
- with any other valid and allowed parameters related to request
- and "address" param is empty

SDL must:
- consider such request as valid
- transfer SendLocation_request without "address" param to HMI
- respond with `<resultCode_received_from_HMI>` to mobile application

_Note:_ in case HMI responded with SAVED resultCode, SDL must transfer "SAVED, success:true" to mobile application

2.
In case mobile application sends SendLocation_request to SDL 
- **without** "address" parameter
- **with both** "longitudeDegrees" and "latitudeDegrees" parameters
- with any other valid and allowed parameters related to request

SDL must:
- consider such request as valid
- transfer SendLocation_request to HMI
- respond with `<resultCode_received_from_HMI>` to mobile application 

_Note:_ in case HMI responded with SAVED resultCode, SDL must transfer "SAVED, success:true" to mobile application

3.
In case mobile application sends SendLocation_request to SDL 
- **with OR without** "address" parameter
- **with just** "longitudeDegrees" OR **with just** "latitudeDegrees" parameters 
- with any other valid and allowed parameters related to request

SDL must:

respond "INVALID_DATA, success:false" to mobile application

4.
In case mobile application sends SendLocation_request to SDL 
- **with OR without** "address" parameter
- **without both** "longitudeDegrees" and "latitudeDegrees" parameters
- with any other valid and allowed parameters related to request

SDL must:

respond "INVALID_DATA, success:false" to mobile application

5.
In case mobile application sends SendLocation_request
- with “deliveryMode” parameter and other valid and allowed parameters
- and “deliveryMode” parameter is allowed by Policies 

SDL must: 
- transfer SendLocation_request to HMI
- respond with `<resultCode_received_from_HMI>` to mobile app

_Note:_ in case HMI responded with SAVED resultCode, SDL must transfer "SAVED, success:true" to mobile application

6.
In case mobile application sends SendLocation_request
- with “deliveryMode” parameter and other valid and allowed parameters
- and “deliveryMode” parameter is NOT allowed by Policies 

SDL must: 
- cut off "deliveryMode" parameter from SendLocation request
- transfer SendLocation without "deliveryMode" parameter to HMI
- respond with `<resultCode_received_from_HMI>` to mobile app with added info: "default value of delivery mode will be used"

_Note:_ In case HMI responded with SAVED resultCode, SDL must transfer "SAVED, success:true" to mobile application

7.
In case mobile application sends SendLocation request to SDL 
- and this request is allowed by Policies for this mobile application
- and "parameters" field is omitted in PolicyTable for this request

SDL must:
- transfer received request with all requested parameters as is to HMI
- respond with `<received_resultCode_from_HMI>` to mobile app

_Note:_ In case HMI responded with SAVED resultCode, SDL must transfer "SAVED, success:true" to mobile application

12.
In case mobile application sends SendLocation_request with
- one or more requested parameters allowed by Policies
- one or more requested parameters disallowed by Policies

and this request is allowed by Policies for this mobile application

SDL must:
- transfer SendLocation with allowed parameters only to HMI
- respond with `<received_resultCode_from_HMI>` + success: (true OR false) + info: " `<param_A>, <param_B>` parameters are disallowed by Policies"

_NOTE:_ 

a. in case with disallowed "deliveryMode" SDL must add to info also: "default value of deliveryMode will be used"

b. in case HMI responded with SAVED resultCode, SDL must transfer "SAVED, success:true" to mobile application

13.
In case mobile application sends SendLocation_request 
- with all requested parameters disallowed by Policies 
- and this request is allowed by Policies for this mobile application

SDL must:

respond with "DISALLOWED, success:false", "info: Requested parameters are disallowed by Policies"

_NOTE:_ In case at least one of the mandatory parameters is disallowed by Policies, SDL must respond with "DISALLOWED, success:false", "info: Requested parameters are disallowed by Policies" to mobile application

14.
In case mobile application sends SendLocation_request
- this request is allowed by Policies for this mobile application
- "parameters" field is empty in PolicyTable for SendLocation RPC

SDL must:
- respond with "DISALLOWED, success:false", "info: Requested parameters are disallowed by Policies"


## Non-Functional requirements

a. The "SendLocation" with "deliveryMode" parameter and with other related parameters must be supported in `<functional_grouping>` section of PolicyTable

```json
     "functional_groupings": 
      "<group_1>": {
        "rpcs": {
          "SendLocation": {
            "hmi_levels": [
              "BACKGROUND",                
              "FULL",
              "LIMITED"                                                                                                          
            ],
        "parameters": [
          "accPedalPosition",
                "longitudeDegrees",
                "latitudeDegrees",
                "locationName",
                "locationDescription",
                "addressLines",
                "phoneNumber"
                "locationImage"
                "deliveryMode"
                "timeStamp"
                "address"
              ]
            },
 ```

c. PoliciesManager must allow all parameters of SendLocation defined in `<functional_grouping>` of PolicyTable for mobile app assigned with this `<functional_grouping>`

## Diagram

SendLocation

![SendLocation](https://github.com/smartdevicelink/sdl_requirements/blob/SendLocation/detailed_docs/accessories/SendLocation_cases.png)


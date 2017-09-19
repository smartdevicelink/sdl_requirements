## Use Case 1: SendLocation

**Main Flow:**

_Pre-conditions:_

a. HMI and SDL are started

b. appID is registered on SDL

_Steps:_

1. appID requests SendLocation with address, longitudeDegrees, latitudeDegrees, deliveryMode and other parameters

_Expected:_

2. SDL validates parameters of the request
3. SDL checks if Navi interface is available on HMI
4. SDL checks if SendLocation is allowed by Policies
5. SDL checks if deliveryMode is allowed by Policies
6. SDL transfers the request with allowed parameters to HMI
7. SDL receives response from HMI
8. SDL transfers response to mobile app

**Alternative flow 1:**

1.a.1. appID requests SendLocation **without** address and **with** longitudeDegrees, latitudeDegrees, deliveryMode and other valid and allowed parameters

1.a.2. SDL transfers the requests to HMI

**Alternative flow 2:**

1.b.1. appID requests SendLocation **with** address, deliveryMode, other parameters **and without** longitudeDegrees  **or** latitudeDegrees **or without both** longitudeDegrees and latitudeDegrees

1.b.2. SDL responds "INVALID_DATA, success:false" to appID and doesn't transfer the request on HMI

**Alternative flow 3:**

1.e.1. appID requests SendLocation **with deliveryMode**, other valid and allowed parameters and **without**  address, latitudeDegrees and longitudeDegrees

1.e.2. SDL responds "INVALID_DATA, success:false" to appID and doesn't transfer the request on HMI

**Alternative flow 4:**

1.f.1. appID requests SendLocation **without** deliveryMode and with other allowed and valid parameters 

1.f.2. SDL transfers the requests to HMI

**Exception 1:**

2.1.a Request is invalid: Wrong json, parameters of wrong type, string parameters with empty values or whitespace as the only symbol, out of bounds, wrong characters, missing mandatory parameters

2.1.b SDL responds INVALID_DATA, success:false

**Exception 2:**

2.2.a "address" parameters is empty

2.2.b SDL transfers SendLocation_request to HMI without "address" parameter

**Exception 3:**

3.1.a Navi interface is not available on HMI

3.1.b SDL respponds with UNSUPPORTED_RESOURCE, success: false to mobile app

**Exception 4:**

3.2.a "sendLocationEnabled": false in HMI navigation capabilities

3.2.b SDL respponds with UNSUPPORTED_RESOURCE, success: false to mobile app

**Exception 5:**

4.1.a SendLocaction is not allowed by Policies

4.1.b. SDL responds DISALLOWED, success:false and doesn't transfer this request to HMI

**Exception 6:**

5.1.a deliveryMode is not allowed by Policies

5.1.b SDL cuts off deliveryMode parameter from application's request and if other parameters are valid, transfers request to HMI without deliverMode

5.1.c SDL transfers received response from HMI to mobile app with added info: "default value of deliveryMode will be used"

**Exception 7:**

6.1.a Some of requested parameters are not allowed by Policies

6.1.b SDL cuts off disallowed by Policies parameters and transfers request to HMI

6.1.c SDL transfers received result code from HMI to mobile app with info: "<param_1>, <param_2> parameters are disallowed by Policies"

**Exception 8:**

6.2.a "parameters" field is omitted in Policy Table for SendLocation RPC

6.2.b SDL transfers request with all valid parameters to HMI

**Exception 9**

6.3.a "parameters" field is empty for SendLocation RPC in Policy Table 

6.3.b SDL responds with DISALLOWED, success:false result code and info: "Requested parameters are disallowed by Policies"

**Exception 10:**

6.4.a All requested paramters are disallowed by Policies

6.4.b SDL responds with DISALLOWED, success:false result code and info: "Requested parameters are disallowed by Policies"

**Exception 11:**

7.1.a SDL received "SAVED" resultCode from HMI

7.1.b SDL trnasfers "SAVED, success:true" to appID

> _[Requirement #24](https://github.com/smartdevicelink/sdl_requirements/issues/24) SendLocation to set a location on the head unit_

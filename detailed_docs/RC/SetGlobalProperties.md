## Use Case 1: SetGlobalProperties: request to set user's location

**Main Flow:**

_Pre-conditions:_

a. mobile device is connected to SDL

b. app is registered with REMOTE_CONTROL appHMIType


_Steps:_

1. App requests to set the value of user's location sending SetGlobalProperties(`userLocation`, `appID`)

_Expected:_

2. SDL forwards the request to HMI
3. SDL saves internally `userLocation` for the `appID`
4. SDL transfers request to HMI
5. SDL receives the response from HMI
6. SDL transfers received response from HMI to the app

**Exceptions:**

1.1 SetGlobalProperties RPC is not valid -> SDL responds INVALID_DATA and doesn't process the request

1.2 SetGlobalProperties RPC is not allowed by policies -> SDL responds DISALLOWED and doesn't process the request


**Alternative flow 1**

1.a.1 App requests to set the value of user location sending SetGlobalProperties without `userLocation` parameter  
1.a.2 SDL does NOT transfer request to HMI  
1.a.3 SDL considers app location as a driver by default and retrieves driver's location from `seatLocationCapabilities` (the fisrt element published by HMI)  
1.a.4 SDL saves internally `userLocation` for the `appID`

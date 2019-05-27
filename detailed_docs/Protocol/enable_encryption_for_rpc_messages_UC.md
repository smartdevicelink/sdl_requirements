## Use case 1: Handling RPCs from mob app before ecrypted RPC service is enabled

**Main Flow**

_Pre-conditions:_

a.	mobile application is registered and running on SDL  
b.  encryption of RPC service 7 is **not enabled**

_Steps:_
1. mob app sends an RPC request that **does not need protection**

_Expected:_  

2. SDL transfers RPC request to HMI  
3. SDL sends respose received from HMI to mob app

**Alt flow 1**  
1.1 mob app sends an RPC request that **needs protection**  
1.2 SDL rejects the request with result code `ENCRYPTION_NEEDED`

_Note: whether RPC needs protection is defined by `encryption_required` flag in within `app_policies`, `function_group`_

## Use case 2: Handling notifications before ecrypted RPC service is enabled

_Pre-conditions:_

a.  mobile application is registered and running on SDL  
b.  encryption of RPC service 7 is **not enabled**

_Steps:_
1. HMI sends a notification that **does not need protection**

_Expected:_  

2. SDL transfers the notification to the mob app

**Alt flow 1**  
1.1 HMI sends a notification that **needs protection**  
1.2 SDL doesn't send the notification to the mob app

## Use Case 3: Handling RPCs from mob app after ecrypted RPC service is enabled

**Main Flow:**

_Pre-conditions:_

a.	mobile application is registered and running on SDL  
b.  encryption of RPC service 7 is **enabled**

_Steps:_
1. mob app sends an **encrypted RPC request** that needs protection

_Expected:_  

2. SDL transfers RPC request to HMI  
3. SDL sends an encrypted response received from HMI

**Alt flow 1**  
1.2.1 mob app sends an **encrypted RPC request** that **does not need protection** protection  
1.2.2 SDL continues processing an encrypted RPC request sending it to HMI  
1.2.3 SDL sends encrypted respose received from HMI to mob app

**Alt flow 2**   
1.3.1 mob app sends an **unencrypted RPC request** that needs protection  
1.3.2 SDL sends unencrypted response with result code `ENCRYPTION_NEEDED` to mob app  

**Alt flow 3**   
1.4.1 mob app sends an **unencrypted RPC request** that **does not need protection**  
1.4.2 SDL continues processing an unencrypted RPC request sending it to HMI  
1.4.3 SDL sends unencrypted respose received from HMI to mob app

## Use case 4: Handling notifications after ecrypted RPC service is enabled
**Main Flow**  

_Pre-conditions:_

a.  mobile application is registered and running on SDL  
b.  encryption of RPC service 7 is **enabled**

_Steps:_
1. HMI sends an encrypted notification that **needs protection**

_Expected:_  

2. SDL transfers encrypted notification to the mob app

**Alt flow 1**  
1.2.1 HMI sends an encrypted notification that **does not need protection**  
1.2.2 SDL transfers encrypted notification to the mob app

**Alt flow 2**  
1.3.1 HMI sends an unencrypted notification that **needs protection**  
1.3.2 SDL transfers unencrypted notification to the mob app

**Alt flow 3**  
1.4.1 HMI sends an unencrypted notification that **does not need protection**  
1.4.2 SDL transfers unencrypted notification to the mob app

## Use case 5: Handling RPCs from mob app after PTU 

a.	mobile application is registered and running on SDL  
b.  encryption of RPC service 7 is **not enabled**

_Steps:_
1. mob app sends an RPC request that **needs protection**
2. SDL rejects the request with result code `ENCRYPTION_NEEDED`
3. PTU brings the missing `encryption_required` parameters for `app_policies` and for the appropriate `function_group`

_Expected:_  
4. SDL sends OnPermissionsChange notification with updated parameters for the RPC to the app  
5. RPC service 7 starts in protected mode  
6. mob app sends an RPC request that **needs protection**  
7. SDL transfers RPC request to HMI  
8. SDL sends encrypted respose received from HMI to mob app
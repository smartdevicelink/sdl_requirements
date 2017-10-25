
## Use Case 1: Handling notifications from HMI during re-connection

**Main Flow:**

_Pre-conditions:_  
a. iOS device is connected over Bluetooth  
b. N applications from this device are registered in SDL  
 
_Steps:_  
 1. User connects the same device over USB
 
 
_Expected:_   
 2. SDL starts reconnection timer  
 3. HMI sends no RPCs to SDL assigned to these N applications  
 4. SDL unregisters the applications right after reconnection timer expires

**Alternative flow 1**  
3.1 HMI sends an RPC to  any of these N applications that need to be reconnected  
3.1.a. SDL stores all the RPCs from HMI internally  
3.1.b. SDL transfers all RPCs from HMI right after corresponding application reconnects 
 
**Alternative flow 2**  
3.2 HMI sends a response to any of these N applications requests that were sent before reconnection  
3.2.a. SDL discards all these responses from HMI

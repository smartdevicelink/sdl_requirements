## Use Case 1: Notification about RC modules allocation during application’s registration

**Main Flow:**

_Pre-conditions:_

a.  Mobile device is connected to SDL

b.	RC functionality is allowed on HMI

c.	All modules are free

d.	RC functionality is allowed by policies

_Steps:_

1.	RC application starts registration

_Expected:_
1. SDL sends OnRCStatus with all modules in `freeModules` and empty array of `allocatedModules` to HMI
2. SDL sends OnRCStatus(allowed = true, freeModules = {}) notification and to registered mobile application

**Alternative flow 1**
b.1 RC functionality is disallowed on HMI
b.2 SDL sends OnRCStatus(allowed = false, freeModules = {}, allocatedModules = {}) notification to registered mobile application

**Alternative flow 2**
1.a.1 Non-RC application starts registration
1.a.2 SDL does not sends OnRCStatus to HMI and non-RC application


## Use Case 2: Notification about RC module allocation after application’s registration

**Main Flow:**

_Pre-conditions:_

a.	Mobile device is connected to SDL

b.	RC functionality is allowed on HMI

c.	All modules are free

d.	RC functionality is allowed by policies

e.	RC app_1 is registered

_Steps:_
1.	RC app_1 sends control request to module_1 (upon a OnButtonPress or SetInteriorVehicleData request)

2. RC app_1 allocates module_1

_Expected:_
3. SDL sends OnRCStatus notification to HMI with module_1 in `allocatedModules` parameter and remaining modules in `freeModules`
4. SDL sends OnRCStatus notification to RC app_1 with (allowed = true), module_1 in `allocatedModules` parameter and remaining modules in `freeModules`

**Alternative flow 1**
1.a.1 RC app_1 already allocates module_1
1.a.2 RC app_1 sends control request to module_1 one more time
1.a.3 SDL does not send OnRCStatus

**Alternative flow 2**
1.b.1 HMI doesn't response or responses with invalid values of parameters
1.b.2 SDL responds with resultCode GENERIC_ERROR to application
1.b.3 SDL does not sends OnRCStatus

## Use Case 3: Notification about deallocation of RC module

**Main Flow:**

_Pre-conditions:_
a.	mobile device is connected to SDL

b.	RC functionality is allowed on HMI

c.	RC functionality is allowed by policies

d.	RC app_1 is registered

e.	RC app_1 allocates module_1

_Steps:_

1. HMI disallows RC functionality

_Expected:_

2. SDL sends OnRCStatus(allowed = false) notification with empty arrays of `allocatedModules`, `freeModules` to RC app_1


**Alternative flow 1**
1.a.1 RC app_1 starts unregistration
1.a.2 RC app_1 successfully unregistered
1.a.3 SDL sends OnRCStatus to HMI with all modules in `freeModules`

**Exception 1.1**
1.a.2.a RC sends invalid json in unregister request
1.a.2.b Unregistration is failed, SDL does not send OnRCStatus to HMI and to RC app_1

**Alternative flow 2**
1.b.1 RC app_1 stops controlling module_1
1.b.2 SDL sends OnRCStatus to HMI with all modules in `freeModules`


## Use Case 4: Notification about change of RC module allocation for multiple apps

**Main Flow:**

_Pre-conditions:_

a.	RC functionality is allowed on HMI

b.	All modules are free

c.	RC functionality is allowed by policies

d.	RC app_1 and RC app_2 are registered

_Steps:_

1.	RC app_1 sends control request to module_1

2.  RC app_1 allocates module_1

_Expected:_

3. SDL sends OnRCStatus
- to HMI (app_1, allocatedModules:module_1; freeModules:module_2, module_3); (app_2, allocatedModules:(); freeModules:module_2, module_3)
- to RC app_1 (allocatedModules:module_1; freeModules:module_2, module_3; allowed = true)
- to RC app_2 (allocatedModules:(); freeModules:module_2, module_3; allowed = true)

**Alternative flow 1**
d.1 In case app_2 is not RC – > SDL sends OnRCStatus notification with all `freeModules` only to HMI and to RC app_1.
d.2 SDL does NOT send OnRCStatus notification to app_2

**Alternative flow 2**
1.a.1 RC app_1 starts unregistration
1.a.2 RC app_1 successfully unregistered
1.a.3 SDL sends OnRCStatus to HMI and to app2 with all modules in `freeModules`

**Exception 1.1**
1.a.2.a RC sends invalid json in unregister request
1.a.2.b Unregistration is failed, SDL does not send OnRCStatus to HMI and to registered apps

**Alternative flow 3**
1.b.1 RC app_2 allocates the module_1
1.b.2 SDL sends OnRCStatus to HMI, RC app_1, RC app_2  with module_1 in `allocatedModules`

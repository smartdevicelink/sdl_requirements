## Functional Requirements

1.
In case
- RC functionality is allowed on HMI
- all RC modules are free

AND RC application sends ButtonPress or SetInteriorVehicleData request

SDL must
send OnRCStatus notification with RC modules in 'freeModules' and 'allocatedModules' parameters to HMI
send OnRCStatus notification with 'allowed = true', 'freeModules' and 'allocatedModules' parameters to mobile application

2.
In case
- RC functionality is disallowed on HMI
- all RC modules are free

AND RC application sends ButtonPress or SetInteriorVehicleData request

SDL must
reject the request and NOT send OnRCStatus notification to HMI and to registered RC application

3.
In case
- RC functionality is allowed on HMI
- all RC modules are free

AND non-RC application sends ButtonPress or SetInteriorVehicleData request

SDL must
NOT send OnRCStatus to HMI and to registered non-RC application

4.
In case
- RC functionality is allowed on HMI
- all RC modules are free
- RC app_1 is registered with SDL

AND RC app_1 allocates module_1

SDL must
send OnRCStatus to HMI (app_1, allocatedModules:module_1; freeModules:module_2, module_3);
send OnRCStatus to RC app_1 (allocatedModules:module_1; freeModules:module_2, module_3; allowed:true);

5.
In case
- RC functionality is allowed on HMI
- RC application is registered with SDL
- RC application allocates module_1

AND RC application unregisters or stops controlling module_1(goes to HMI level NONE)

SDL must
send OnRCStatus to HMI and to RC application with module_1 in `freeModules`
send OnRCStatus to RC application with module_1 in `freeModules` and allowed:true

6.
In case
- RC functionality is allowed on HMI
- all RC modules are free
- RC app_1 is registered with SDL

AND RC app_2 starts registration

SDL must
send and OnRCStatus notification with 'allowed = true' and all modules in `freeModules` parameter to RC app_2

7.
In case
- RC functionality is allowed on HMI
- all RC modules are free
- RC app_1 and RC app_2 are registered with SDL

AND RC app_1 allocates module_1

SDL must

send OnRCStatus to HMI (app_1, allocatedModules:module_1; freeModules:module_2, module_3); (app_2, allocatedModules:(); freeModules:module_2, module_3);

send OnRCStatus to RC app_1 (allocatedModules:module_1; freeModules:module_2, module_3; allowed:true);

send OnRCStatus to RC app_2 (allocatedModules:(); freeModules:module_2, module_3; allowed:true)


 8.
 In case
- RC functionality is allowed on HMI, with allowed or without specified access mode
- RC app_1 and RC app_2 are registered with SDL
- RC app_1 is in control of module_1
- module_2, module_3 are free

AND RC app_2 allocates module_1

SDL must
SDL send OnRCStatus to HMI (app_1, allocatedModules:(), freeModules:module_2, module_3); (app_2, allocatedModules:module_1; freeModules:module_2, module_3)
SDL send OnRCStatus to RC app_1 (allocatedModules:(); freeModules:module_2, module_3; allowed:true)
SDL send OnRCStatus to RC app_2 (allocatedModules:module_1; freeModules:module_2, module_3; allowed:true)

9.
 In case
- RC functionality is allowed on HMI
- RC app_1 is registered with SDL
- RC app_1 allocates module_1

AND RC app_2 is registering with PT with revoked allocated module_1

SDL must
deallocates module_1 from RC app_1
send OnRCStatus to HMI with all modules in `freeModules`
send OnRCStatus to app1 and app2 with all modules in `freeModules` and with allowed:true

10.
  In case
- RC functionality is allowed on HMI
- RC app_1 is registered with SDL

AND PTU performed RC app_1 has no permission for OnRCStatus RPC

SDL must
send OnRCStatus to HMI with module_1 in `freeModules`
not send OnRCStatus to RC app_1

## Diagrams

OnRCStatus
![OnRCStatus](https://github.com/smartdevicelink/sdl_requirements/blob/master/detailed_docs/accessories/OnRCStatus.png)

## Non-Functional requirements

### Additions to Mobile_API
```
<element name="OnRCStatusID" value="32784" hexvalue="801A" />

<function name="OnRCStatus" messagetype="notification">
  <description>Issued by SDL to notify the application about remote control status change on SDL</description>
     <param name="allowed" type="Boolean" mandatory="false" >
       <description>If "true" - RC is allowed; if "false" - RC is disallowed.
                    Not present in the notification in case by module allocation/deallocation.
                    Present in notification in cases enabling/disabling RC or RC-app registration.
      </description>
     </param>
      <param name="allocatedModules" type="ModuleData" minsize="0" maxsize="100" array="true" mandatory="true">
        <description>Contains a list (zero or more) of module types that are allocated to the application.</description>
      </param>
      <param name="freeModules" type="ModuleData" minsize="0" maxsize="100" array="true" mandatory="true">
        <description>Contains a list (zero or more) of module types that are free to access for the application.</description>
      </param>
    </function>
```

### Additions to HMI_API
```
<function name="OnRCStatus" messagetype="notification">
  <description>Issued by SDL to notify HMI about remote control status change on SDL</description>
  <param name="appID" type="Integer" mandatory="true">
    <description>ID of selected application.</description>
  </param>
  <param name="allocatedModules" type="ModuleData" minsize="0" maxsize="100" array="true" mandatory="true">
    <description>Contains a list (zero or more) of module types that are allocated to the application.</description>
  </param>
  <param name="freeModules" type="ModuleData" minsize="0" maxsize="100" array="true" mandatory="true">
    <description>Contains a list (zero or more) of module types that are free to access for the application.</description>
  </param>
</function>
```

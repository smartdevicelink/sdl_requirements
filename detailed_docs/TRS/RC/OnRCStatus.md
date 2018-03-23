## Functional Requirements  

1.
In case
- RC functionality is allowed on HMI  
- all RC modules are free

AND RC application sends ButtonPress or SetInteriorVehicleData request 

SDL must  
send OnRCStatus notification to HMI and to mobile application  
with all RC modules in `freeModules` parameter


2.
In case   
- RC functionality is disallowed on HMI  
- all RC modules are free

AND RC application sends ButtonPress or SetInteriorVehicleData request 

SDL must  
NOT sends OnRCStatus to HMI and registered application  

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
- RC application is registered with SDL

AND RC application allocates module_1  

SDL must  
send OnRCStatus to HMI and to registered RC application 
with module_1 in parameter `allocatedModules` and remained modules in `freeModules`

5. 
In case  
- RC functionality is allowed on HMI 
- RC application is registered with SDL 
- RC application allocates module_1

AND RC application unregisters or stops controlling module_1(goes to HMI level NONE)

SDL must  
send OnRCStatus to HMI and to RC application with module_1 in `freeModules`

6.  
In case  
- RC functionality is allowed on HMI   
- all RC modules are free
- RC app_1 is registered with SDL

AND RC app_2 starts registration

SDL must  
Send OnRCStatus notification to HMI and to RC app_1, RC app_2 with all modules in `freeModules` parameter.


7. 
In case  
- RC functionality is allowed on HMI 
- all RC modules are free
- RC app_1 and RC app_2 are registered with SDL

AND RC app_1 allocates module_1  

SDL must  
send OnRCStatus to HMI and to RC app_1, RC app_2 with module_1 in section `allocatedModules`  
and remained modules in `freeModules`

 8.
 In case  
- RC functionality is allowed on HMI 
- RC app_1 and RC app_2 are registered with SDL
- RC app_1 is in control of module_1 

AND RC app_2 allocates module_1

SDL must  
sends OnRCStatus to HMI and to RC app_1, RC app_2 with module_1 in section `allocatedModules`  


9.  
 In case  
- RC functionality is allowed on HMI 
- RC app_1 is registered with SDL 
- RC app_1 allocates module_1 

AND RC app_2 is registering with PT with revoked allocated module_1  

SDL must  
deallocates module_1 from RC app_1  
send OnRCStatus to HMI and to app1 and app2 with all modules in freeModules  

10.  
  In case  
- RC functionality is allowed on HMI  
- RC app_1 is registered with SDL 

AND PTU performed RC app_1 has no permission for OnRCStatus RPC  

SDL must  
send OnRCStatus to HMI with module_1 in `freeModules`  
not send OnRCStatus to RC app_1 


## Non-Functional requirements  

### Additions to Mobile_API
```
<element name="OnRCStatusID" value="32784" hexvalue="801A" />

<function name="OnRCStatus" messagetype="notification">
  <description>Issued by SDL to notify the application about remote control status change on SDL</description>
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

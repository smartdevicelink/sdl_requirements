## Functional requirements  

1.  
In case  
mobile app sends RegisterAppInterface_request to SDL  
and SDL checks the existence of apps icon at `<AppIconsFolder>`  
and the apps icon exists at `<AppIconsFolder>` (value of [ini.file](https://github.com/smartdevicelink/sdl_core/blob/develop/src/appMain/smartDeviceLink.ini))  

SDL must  
- respond RegisterAppInterface (`<successfull_resultCode>`, "iconResumed" = true, params) to mobile app (in case of no other failures)  
- provide the `<icon>` via OnAppRegistered_notification to HMI
 

2. 
In case  
mobile app sends RegisterAppInterface_request to SDL  
and the apps icon does NOT exist at `<AppIconsFolder>`  

SDL must
- respond RegisterAppInterface (SUCCESS, "iconResumed" = false, params) to mobile app
- send OnAppRegistered_notifcation to HMI without `<icon>` parameter


## Non-functional requirements  
MOBILE_API change
```
<function name="RegisterAppInterface" functionID="RegisterAppInterfaceID" messagetype="response">   
 <description>The response to registerAppInterface</description>   
 
 ...   
<param name="iconResumed" type="Boolean" mandatory="true">
<description>Existence of apps icon at system.    
 If true, apps icon was resumed at system.     
 If false, apps icon is not resumed at system</description>`    
 </param>    
 </function>
 ```  

## Diagram

App Icon Resumption

![App Icon Resumption](https://github.com/smartdevicelink/sdl_requirements/blob/master/detailed_docs/accessories/App_Icon_Resumption.png)
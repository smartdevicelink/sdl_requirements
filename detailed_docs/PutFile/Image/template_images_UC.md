## Use Case 1: Mobile app sends request with template image

**Main Flow:**  
_Pre-conditions:_  
a. SDL and HMI are started  
b. Mobile application is successfully registered on SDL  
c. PNG image was successfully added to the system by mobile application 

_Steps:_  
1. Mobile application sends a request with uploaded PNG image and `isTemplate` flag set to 'True'
_Expected:_  
2. SDL validates received RPC and params - valid  
3. SDL checks RPC by Policies - allowed 
4. SDL transfers the request with `isTemplate` flag set to 'True' to HMI 
5. HMI loads the proper image pattern
6. HMI extracts alpha channel from the template image
7. HMI applies the alpha channel to the image pattern
8. HMI displays this newly generated (re)colored image instead of the uploaded one  
9. SDL receives response from HMI  
10. SDL transfers response to mobile application

**Alternative flow 1**  
1.a.1. Mobile application sends a request with **not** PNG image and 'isTemplate' flag set to 'True'  
1.a.2. SDL validates received RPC and params - valid  
1.a.3. SDL checks RPC by Policies - allowed  
1.a.4. SDL transfers the request with `isTemplate` flag set to 'True' to HMI  
1.a.5. HMI should ignore `isTemplate = true`  
1.a.6. HMI does not generate (re)colored image  
1.a.7. HMI sends warning response with message '`isTemplate` was ignored because the provided image does not include alpha/transparent channel.' to SDL  
1.a.8. SDL transfers response to mobile application  

**Alternative flow 2**  
1.b.1. Mobile application sends a request with uploaded PNG image and `isTemplate = true` and STATIC image type in Image structure  
1.b.2. SDL validates received RPC and params - valid  
1.b.3. SDL checks RPC by Policies - allowed  
1.b.4. SDL transfers the request with `isTemplate` flag set to 'True' to HMI   
1.b.5. HMI checks for the list of static images  
1.b.6. HMI finds that it does not support that particular image  
1.b.7. HMI sends UNSUPPORTED_RESOURCE response to SDL  
1.b.8. SDL transfers response to mobile application

**Alternative flow 3**  
1.c.1. Mobile application sends a request with uploaded PNG image and `isTemplate` flag set to 'False'  
1.c.2. SDL validates received RPC and params - valid  
1.c.3. SDL checks RPC by Policies - allowed  
1.c.4. SDL transfers the request with `isTemplate` flag set to 'False' to HMI  
1.c.5. HMI does not generate (re)colored image  
1.c.6. SDL receives response from HMI  
1.c.7. SDL transfers response to mobile application

**Exception 1:**  
2.1.a Request is invalid: Wrong json, parameters of wrong type, string parameters with empty values or whitespace as the only symbol, out of bounds, wrong characters, missing mandatory parameters  
2.1.b SDL responds INVALID_DATA, success:false to mobile app

**Exception 2:**  
3.1.a Request is disallowed by Policies  
3.1.b. SDL responds DISALLOWED, success:false and doesn't transfer this request to HMI  

**_List of impacted requests:_**  
Show  
SetGlobalProperties  
SendLocation  
ShowConstantTBT  
ScrollableMessage  
AlertManeuver  
UpdateTurnList  
Alert  
CreateInteracrionChoiceSet  
AddCommand  
PerformInteraction 
## Use Case 1: Mobile app is adding a sub menu with referenced image

**Main Flow:**

_Pre-conditions:_

a. SDL amd HMI are started

b. Mobile application is registered on SDL  

c. An image was successfully added to the system by mobile application


_Steps:_
1. Application requests to add a sub menu to the specified application's menu with the referenced image

_Expected:_

2. SDL checks if such request is valid
3. SDL checks if such request is allowed by Policies for this app  
4. SDL transfers the request to HMI    
5. HMI displays submenu with the referenced image
5. SDL receives response from HMI
6. SDL transfers response to mobile application

**Exception 1:** 

1.a.1. The image referenced by the application for displaying on the submenu is **invalid**  
1.a.2. SDL transfers the request with other valid params to HMI  
1.a.3. HMI response with warning information that the SubMenu was added but no image was displayed  
1.a.4. SDL transfers response to mobile application  


**Exception 2:**  
1.b.1. The image referenced by the application for displaying on the submenu **does NOT exist** in the system  
1.b.2. SDL transfers the request to HMI   
1.b.3. HMI response with warning information that the SubMenu was added but no image was displayed  
1.b.4. SDL transfers response to mobile application

**Exception 3:**  
1.c.1. Application requests to add a sub menu to the specified application's menu **without** image  
1.c.2. SDL transfers the request with other valid params to HMI  
1.c.3. HMI responses to SDL and displays submenu without image  
1.c.4. SDL transfers response to mobile application  


**Exception 4:**  
2.1.Request is invalid: Wrong json, parameters of wrong type, string parameters with empty values or whitespace as the only symbol, out of bounds, wrong characters, missing mandatory parameters  
2.1.SDL responds INVALID_DATA, success:false to mobile application


**Exception 5:**  
3.1 Request is disallowed by policies  
3.2 SDL responds DISALLOWED, success:false and doesn't transfer this request to HMI
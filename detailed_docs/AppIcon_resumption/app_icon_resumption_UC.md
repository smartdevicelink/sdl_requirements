## Use Case 1: AppIcon resumption across application registration request

**Main Flow:**   

_Pre-conditions:_  
a. SDL and HMI are started  
b. Mobile application previously successfully set its app icon AND the app icon exists on the file system 
c. Mobile application re-registeres   

_Steps:_  
1. Mobile application sends registration request  

_Expected:_  
2. SDL checks the existence of apps `icon` at AppIconsFolder  
3. The apps `icon` exists at AppIconsFolder
3. SDL sends a response of success to mobile application with iconResumed flag on   
4. SDL sends notification with path to `icon` in AppIconsFolder to HMI  
5. The application icon resumed in the system and is displayed on the HMI


**Alternative flow 1:**  
b.1. Mobile application had NOT previously set an app icon successfully and / or the file does not exist on the head unit  

_Steps:_  
1. Mobile application sends registration request  

_Expected:_  

2. SDL checks the existence of apps icon at AppIconsFolder  
3. The apps icon does NOT exist at AppIconsFolder  
4. SDL sends a response of success to mobile application with iconResumed flag off  
5. SDL sends notification without "icon" parameter to HMI 
6. Default app icon is displayed on the HMI

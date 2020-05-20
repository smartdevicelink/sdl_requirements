## Functional Requirements 
 
#### 1. ResultCodes

1.  
In case mobile application sends valid and allowed by Policies AddSubMenu_request to SDL 

SDL must:
  
- transfer AddSubMenu_request to HMI  
- respond with `<resultCode>` received from HMI to mobile application

2.  
In case mobile application sends a sub menu with "menuName" the same as one of subMenus created before for this application  

SDL must:  
- respond with "resultCode "DUPLICATE_NAME" and general result succcess="false"  
- not transfer this RPC to HMI


#### 2. AddSubMenuIcon

3.  
In case 
mobile application sends valid AddSubMenu_request to SDL with `<menuIcon>` parameter  

and the requested `<menuIcon>` exists at app's sub-directory of AppStorageFolder (the value defined at .ini file)


SDL must:  
- transfer AddSubMenu (`<menuIcon>`, other params)_request to HMI  
- respond with `<resultCode>` received from HMI to mobile application

4.  
In case  
mobile application sends valid AddSubMenu_request to SDL **without** `<menuIcon>` parameter

SDL must:  
- transfer AddSubMenu (other params)_request to HMI (thus, without `<menuIcon>`)  
- respond with `<resultCode>` received from HMI to mobile application 

5.  
In case  
mobile app sends valid AddSubMenu_request to SDL with:
-> valid `<menuIcon>` parameter
and the requested `<menuIcon>`does NOT exist at app's sub-directory of "AppStorageFolder" (the value from .ini file)

SDL must:  
- transfer AddSubMenu (`<menuIcon>`, other params)_request to HMI  
- respond with `<resultCode>` received from HMI + info: "Reference image(s) not found" 

## Non-Functional Requirements
### Mobile API additions

```xml
<function name="AddSubMenu" functionID="AddSubMenuID" messagetype="request">
   :
   <param name="menuIcon" type="Image" mandatory="false" />
</function>
```

### HMI API additions

```xml
<function name="AddSubMenu" messagetype="request">
   :
   <param name="menuIcon" type="Common.Image" mandatory="false" />
</function>
```
## Diagram

AddSubMenu
![AddSubMenu](detailed_docs/accessories/AddSubMenu.png)  

AddSubMenu_menuIcon
![AddSubMenu_menuIcon](detailed_docs/accessories/AddSubMenu_menuIcon.png)
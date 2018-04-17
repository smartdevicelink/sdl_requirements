## Functional requirements  

1.  
In case  
PNG image was added via PutFile

and mob app sends RPC with uploaded image and `isTemplate = true` in Image structure

SDL must  

transfer RPC with Image(isTemplate = true) to HMI  
respond with `<resultCode_received_from_HMI>` to mobile app

_List of impacted RPCs:_  
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


**_Note:_**  
If some type of image isn't supported on HMI (any static or dynamic) and SDL knows that this type isn't supported on HMI (e.g. from Capabilities), SDL anyway should send image data in the request (filename, type etc) to HMI.

2.	
In case

PNG image was added via PutFile

and mob app SDL sends RPC with uploaded image and `isTemplate = false` in Image structure

SDL must  

- transfer RPC with Image(isTemplate = false) to HMI  
- respond with `<resultCode_received_from_HMI>` to mobile app  

3.  
In case

PNG image was added via PutFile

and mob app sends RPC with uploaded image and **without** `isTemplate` in Image structure

SDL must  

- transfer RPC without `isTemplate` param to HMI  
- respond with `<resultCode_received_from_HMI>` to mobile app

4.  
In case

PNG image was added via PutFile

and mob app sends RPC with uploaded image and `isTemplate = 123` **(wrong type)** in Image structure

SDL must  

respond INVALID_DATA to mobile application  

5.  
In case  

PNG image was added via PutFile

and mob app sends RPC with uploaded image and `isTemplate = true`
and `imageType=STATIC` in Image structure  

SDL must  

- transfer RPC with Image(isTemplate = true) to HMI 
- transfer RPC (UNSUPPORTED_RESOURCE,success:false) response from HMI to mob app

6.  
In case

**not** PNG image was added via PutFile  

and mob app sends RPC with uploaded image and `isTemplate = true` in Image structure  

SDL must  

- transfer RPC with Image(isTemplate = true) to HMI 
- transfer response RPC (WARNING,success=true) + the "message" of the kind "`isTemplate` was ignored because the provided image does not include alpha/transparent channel" to mob app
  

## Non-functional requirements 
The API change on the mobile and HMI is the following:  

```
<struct name="Image">
 <param name="value" minlength="0" maxlength="65535" type="String"></param>
 <param name="imageType" type="ImageType"></param>
 <param name="isTemplate" type="Boolean" mandatory="false"></param> <!-- NEW PARAMETER -->
</struct>  
```  

## Diagram

[Mobile app request with `isTemplate = true`](https://github.com/smartdevicelink/sdl_requirements/master/detailed_docs/accessories/RPC_Image_isTemplate.png)
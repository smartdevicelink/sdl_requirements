## Functional requirements

1.   
In case

Mobile application sends a PutFile "Single Frame" with a known counted checksum to the SDL    

SDL must  
- receive PutFile "Single Frame"  
- verify the counted checksum from the app (by comparing actual and received checksum) 
- respond the app  with result code "SUCCESS"    

2.   
In case  

Mobile application sends a PutFile "Single Frame" with a known wrong counted checksum to the SDL  

SDL must  
- receive PutFile "Single Frame"  
- verify the counted checksum from the app 
- respond with result code "CORRUPTED_DATA"  

3.   
In case  

Mobile application sends a PutFile "Multiple Frame" with a known counted checksum to the SDL.  

SDL must
- receive PutFile "Multiple Frame" 
- verify the counted checksum from the app
- respond with result code "SUCCESS"

4.  
In case  

Mobile application sends a PutFile "Multiple Frame" with a incorrect counted checksum to the SDL  

SDL must  
- receive PutFile "Multiple Frame"  
- verify the counted checksum from the app   
- respond with result code "CORRUPTED_DATA"

5. 
In case  

Mobile application sends a PutFile without parameter `crc` to the SDL  

SDL must
- receive PutFile without parameter `crc`  
- upload data  
- respond with resultCode: "SUCCESS"

6.   
In case  

Mobile application sends file via PutFile chunks  
and SDL process some part of chunks with success result code  
and one chunk came with wrong crc value and SDL responds with result code "CORRUPTED_DATA" to mobile app  

SDL must  
- response "CORRUPTED_DATA"  
- process all other chunks with resultCode "INVALID_DATA"

7.  
In case  

Mobile application sends a PutFile "Single Frame" with a negative (not valid) checksum to the SDL  

SDL must  
- SDL receive putfile "Single Frame" and counted checksum 
- respond with result code "INVALID_DATA"

8.  
In case  

Mobile application sends a PutFile "Multiple Frame" with a negative (not valid) checksum to the SDL  

SDL must  
- SDL receive putfile "Multiple Frame" and counted checksum  
- respond with result code "INVALID_DATA"

9.  
In case  

Mobile application sends a putfile "Single Frame" with a more than maxvalue (not valid) checksum to the SDL  

SDL must  
- receive putfile "Single Frame" and counted checksum 
- respond with result code "INVALID_DATA" 

10.  
In case  

Mobile application sends a putfile "Multiple Frame" with a more than maxvalue (not valid) checksum to the SDL  

SDL must  
- receive putfile "Multiple Frame" and counted checksum  
- respond with result code "INVALID_DATA"


11.  
In case  

Mobile application sends a PutFile with correct checksum  
and some bytes of the data were corrupted  

SDL must  
- receive PutFile  
- verify counted checksum  
- respond with result code "CORRUPTED_DATA"  

## Non-functional requirements  
Addition to MOBILE API:
```
<enum name="Result" internal_scope="base">
  <element name="CORRUPTED_DATA">
    <description>The data sent failed to pass CRC check in receiver end</description>
  </element>
</enum>


<function name="putFile" functionID="putFileID" messagetype="request">
  [...]
  <param name="crc" type="Integer" minvalue="0" maxvalue="4294967295" mandatory="false">
    <description> Additional CRC32 checksum to protect data integrity up to 512 Mbits . </description>
  </param>
  [...]
</function>


<function name="putFile" functionID="putFileID" messagetype="response">
  [...]
  <param name="resultCode" type="Result" platform="documentation">
      <description>See Result</description>
      <element name=" CORRUPTED_DATA "/>
  </param>
  [...]
</function>
```

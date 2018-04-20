## Functional requirements

1. =1  
In case

Mobile application sends a PutFile "Single Frame" with a known counted checksum to the SDL    

SDL must  
- receive PutFile "Single Frame"  
- verify the counted checksum from the app  
- respond the app  with result code "SUCCESS"    

2. =2  
In case  

Mobile application sends a PutFile "Single Frame" with a known wrong counted checksum to the SDL  

SDL must  
- receive PutFile "Single Frame" and  
- verify the counted checksum from the app 
- respond with result code "CORRUPTED_DATA"  

3. = 5  
In case  

Mobile application sends a PutFile "Multiple Frame" with a known counted checksum to the SDL.  

SDL must
- receive PutFile "Multiple Frame" 
- verify the counted checksum from the app
- respond with result code "SUCCESS"

4. = 6  
In case  

Mobile application sends a PutFile "Multiple Frame" with a incorrect counted checksum to the SDL  

SDL must  
- receive PutFile "Multiple Frame"  
- verify the counted checksum from the app   
- respond with result code "CORRUPTED_DATA"

5.  = 10
In case  

Mobile application sends a PutFile without parameter “crc” to the SDL  

SDL must
- receive PutFile without parameter “crc”  
- upload data  
- respond with resultCode: "SUCCESS"

6. =  
In case  

Mobile application sends file via PutFile chunks  
and SDL process some part of chunks with success result code  
and one chunk came with wrong crc value and SDL responds with result code "CORRUPTED_DATA" to mobile app  

SDL must  
response "CORRUPTED_DATA"  
and process all other chunks with resultCode "INVALID_DATA"

7.  
In case  
Mobile application sends a PutFile "Single Frame" with a negative (not valid) checksum to the SDL  
SDL must  
SDL receive putfile “Single Frame” and counted checksum from the Mobile app and  
respond with result code "INVALID_DATA"

8.  
In case  
Mobile application sends a PutFile "Multiple Frame" with a negative (not valid) checksum to the SDL  
SDL must  
SDL receive putfile “Multiple Frame” and counted checksum from the Mobile app and  
respond with result code "INVALID_DATA"

9.  
In case  
The mobile application sends a putfile “Single Frame” with a more than maxvalue (not valid) checksum to the SDL  
SDL must  
SDL receives putfile “Single Frame” and counted checksum from the Mobile app  
and respond with result code "INVALID_DATA" 

10.  
In case  
The mobile application sends a putfile “Multiple Frame” with a more than maxvalue (not valid) checksum to the SDL  
SDL must  
SDL receives putfile “Multiple Frame” and counted checksum from the Mobile app  
and respond with result code "INVALID_DATA"


11.  
In case  
Mobile application sends a PutFile with correct checksum  
And some bytes of the data were corrupted  
SDL must  
Receive PutFile, verify counted checksum and respond with result code "CORRUPTED_DATA"  

## Non-functional requirements  

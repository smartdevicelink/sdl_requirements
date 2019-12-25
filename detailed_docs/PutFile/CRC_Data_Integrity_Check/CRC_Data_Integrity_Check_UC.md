## Use Case 1: Processing putFile request with CRC32 checksum

_Pre-conditions:_
a. SDL and HMI are started  
b. Mobile application is successfully registered and running on SDL 

_Steps:_  

1. Mobile application sends a request to uploade a file as chunks to SDL  
2. The request has known correct counted checksum  

_Expected:_ 

3. SDL verifies the counted checksum from the mobile application - valid 
4. SDL responds with successful result code to mobile application  

**Exception 1:**  

2.1.1 The request has known wrong counted checksum 
2.1.2 SDL responds with result code "CORRUPTED_DATA" to mobile application  

**Exception 2:**  
2.2.1 The request has negative (not valid) checksum  
2.2.2 SDL responds with result code "INVALID_DATA" to mobile application  

**Exception 3:**  
2.3.1 The request has `value` more than `maxvalue`  
2.3.2 SDL responds with result code "INVALID_DATA" to mobile application  

**Exception 4:**  
2.4.1 The request has no counted checksum  
2.4.2 SDL uploads data and responds with successful result code to mobile application          

**Alternative flow 1**  
2.a.1 SDL processes some parts of chunks successfully and one chunk comes with wrong checksum value  
2.a.2 SDL responds with result code "CORRUPTED_DATA" to mobile application  
2.a.3 SDL responds with result code "INVALID_DATA" if mobile application sends further chanks 

**Alternative flow 2** 
2.b.1 The request has correct checksum and some bytes of the data were corrupted  
2.b.2 SDL responds with result code "CORRUPTED_DATA" to mobile application   
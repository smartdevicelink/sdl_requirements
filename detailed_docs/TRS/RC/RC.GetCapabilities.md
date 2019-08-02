## Functional Requirements

1.
In case

SDL sends Interface.GetCapabilities_request to HMI (<Interface> = VR, TTS, UI, Buttons, RC)  
and HMI does not provide capabilities related to requested Interface or provides capabilities of invalid values
 

SDL must  
use the default capabilities stored at HMI_capabilities.json file

2.
In case 
SDL sends RC.GetCapabilities request to HMI
and HMI provides valid capabilities related to requested Interface

SDL must  
use the provided HMI capabilities and save them internally
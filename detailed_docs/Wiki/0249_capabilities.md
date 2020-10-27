## Feature 1: As a mobile application, I want to get HMI capabilities from SDL after Ignition OFF/ON cycle so that I can interact with the HMI efficiently

**Background**

**Given** App requests HMI Capabilities with one of the following RPC
  * UI.GetSupportedLanguages
  * UI.GetCapabilities
  * VR.GetSupportedLanguages
  * VR.GetCapabilities
  * TTS.GetSupportedLanguages
  * TTS.GetCapabilities
  * Buttons.GetCapabilities
  * VehicleInfo.GetVehicleType
  * RC.GetCapabilities
  * UI.GetLanguage
  * VR.GetLanguage
  * TTS.GetLanguage
#### Scenario 1: SDL Provides HMI capabilities persisted in `HMICapabilitiesCacheFile`

**Given** App registers after Ignition OFF/ON cycle

**When** App requests HMI Capabilities

**Then** SDL checks `HMICapabilitiesCacheFile` in `AppStorageFolder` in [smartDeviceLink.ini](https://github.com/smartdevicelink/sdl_core/blob/master/src/appMain/smartDeviceLink.ini)  
**And** SDL provides HMI Capabilities persisted in `HMICapabilitiesCacheFile` in the corresponding response.

---

#### Scenario 2: SDL creates `HMICapabilitiesCacheFile` in case it is not present in `AppStorageFolder` in _SmartDeviceLink.ini_ 

**When** SDL checks `HMICapabilitiesCacheFile` and variable is null/empty in [smartDeviceLink.ini](https://github.com/smartdevicelink/sdl_core/blob/master/src/appMain/smartDeviceLink.ini)  

**Then** SDL must create `HMICapabilitiesCacheFile` file  

---

#### Scenario 3: SDL requests HMI capabilities missing in `HMICapabilitiesCacheFile` from HMI

**When** Any capability requested by an app is missing in `HMICapabilitiesCacheFile`

**Then** SDL must default back to `hmi_capabilities.json` file for corresponding capability  
**And** SDL should send requests to get these capabilities from HMI

---

#### Scenario 4: SDL writes capabilities data received from HMI to `HMICapabilitiesCacheFile` file 

**Given**  Receiving HMI response for any of the UI/VR/TTS/Buttons/VehicleInfo/Navigation/RC capabilities

**When** SDL checks if `HMICapabilitiesCacheFile` file already has the data set received in HMI response` and `HMICapabilitiesCacheFile` file does not have the data set received in HMI response

**Then** SDL writes the HMI response data to `HMICapabilitiesCacheFile` file  
**And** SDL does not send corresponding requests to get HMI capabilities to HMI for subsequent ignition cycles, follow the rest of launch sequence requests/notifications as applicable.  
**And** SDL uses this new data set for apps/SDL Core communication until ignition cycle is performed.

---

#### Scenario 5:  SDL continues launching sequence if capabilities sent fom HMI are already present in `HMICapabilitiesCacheFile` file

**Given**  Receiving HMI response for any of the UI/VR/TTS/Buttons/VehicleInfo/Navigation/RC capabilities

**When** `HMICapabilitiesCacheFile` file already has the data set received in HMI response

**Then** SDL does not overwrite the response to `HMICapabilitiesCacheFile` file  
**And** SDL does not send corresponding request to get HMI capabilities to HMI for subsequent ignition cycles  
**And** SDL follows the rest of the launch sequence

---

#### Scenario 6: SDL overwrites contents of received Dynamic RPC in hmi_capabilities_cache.json file

**Given** Mobile application already obtained HMI capabilities in the ignition cycle

**When** HMI sends a Dynamic RPC with changes to capabilities

**Then** Core must overwrite contents of received notifications in `HMICapabilitiesCacheFile` file and use this data until ignition cycle is performed

**_Dynamic RPCs:_**
  * UI.OnLanguageChange
  * VR.OnLanguageChange
  * TTS.OnLanguageChange

---

#### Scenario 7: SDL deletes `HMICapabilitiesCacheFile` file when a master reset is performed

**Given** All HMI Capabilities are presented in `HMICapabilitiesCacheFile` 

**When** HMI sends OnExitAllApplications with reason `MASTER_RESET`

**Then** SDL Core must delete `HMICapabilitiesCacheFile` in AppStorageFolder in [smartDeviceLink.ini](https://github.com/smartdevicelink/sdl_core/blob/master/src/appMain/smartDeviceLink.ini)

## Feature 2: As a mobile application, I want to get actual HMI capabilities from SDL after HMI SW update

### Scenario 1: SDL regenerates `HMICapabilitiesCacheFile` file when system SW version changes

**Given** App sends RAI after HMI SW update 

**When** SDL's local `ccpu_version` does not match with received from BC.GetSystemInfo(`ccpu_version`)

**Then** SDL must delete `HMICapabilitiesCacheFile`  
**And** SDL must send `GetCapabilities` requests for each interface (VR, TTS, UI, etc) to HMI and persist the responses received from HMI to `HMICapabilitiesCacheFile`  
**And** SDL must set `IsHMICooperating` to true when HMI has responded to all the HMI capabilities requests  
**And** SDL must respond to all pending RAI requests from mobile applications

---

### Scenario 2: SDL does not persist capabilities after SW update if HMI doesn't respond to `GetCapabilities` requests 
**Given** SDL sends `GetCapabilities` requests for each interface (VR, TTS, UI, etc) to HMI

**When** HMI does not send response for one ore more `GetCapabilities` requests or HMI sends errors

**Then** SDL must fall back to default HMI Capabilities  
**And** SDL must NOT persist those capabilities in cache  
**And** SDL must set `IsHMICooperating` to true  
**And** SDL must respond to all pending RAI requests from mobile applications

## Resources
#### Diagrams:
**_Static RPCs:_**
  * [UI.GetSupportedLanguages](https://github.com/KhrystynaDubovyk/sdl_requirements/blob/master/detailed_docs/accessories/UI_GetSupportedLanguages.png)
  * [UI.GetCapabilities](https://github.com/KhrystynaDubovyk/sdl_requirements/blob/master/detailed_docs/accessories/UI_GetCapabilities.png)
  * [TTS.GetSupportedLanguages](https://github.com/KhrystynaDubovyk/sdl_requirements/blob/master/detailed_docs/accessories/TTS_GetSupportedLanguages.png)
  * [TTS.GetCapabilities](https://github.com/KhrystynaDubovyk/sdl_requirements/blob/master/detailed_docs/accessories/TTS_GetCapabilities.png)
  * [Buttons.GetCapabilities](https://github.com/KhrystynaDubovyk/sdl_requirements/blob/master/detailed_docs/accessories/Buttons_GetCapabilities.png)
  * [RC.GetCapabilities](https://github.com/KhrystynaDubovyk/sdl_requirements/blob/master/detailed_docs/accessories/RC_GetCapabilities.png)
  * [UI.GetLanguage](https://github.com/KhrystynaDubovyk/sdl_requirements/blob/master/detailed_docs/accessories/UI_GetLanguage.png)
  * [BC.GetSystemInfo-CCPU version changed](https://github.com/KhrystynaDubovyk/sdl_requirements/blob/master/detailed_docs/accessories/GeySystemInfo_ccpu_versionChanged.png)
  * [BC.GetSystemInfo-CCPU version did not change](https://github.com/KhrystynaDubovyk/sdl_requirements/blob/master/detailed_docs/accessories/GeySystemInfo_ccpu_versionSame.png)

**_Dynamic RPCs:_**
  * [UI.OnLanguageChange](https://github.com/KhrystynaDubovyk/sdl_requirements/blob/master/detailed_docs/accessories/UI_OnLanguageChange.png)
  * [VR.OnLanguageChange](https://github.com/KhrystynaDubovyk/sdl_requirements/blob/master/detailed_docs/accessories/VR_OnLanguageChange.png)

#### Proposal: [SDL-0249 Persisting HMI Capabilities specific to headunit](https://github.com/smartdevicelink/sdl_evolution/blob/master/proposals/0249-Persisting-HMI-Capabilities-specific-to-headunit.md)

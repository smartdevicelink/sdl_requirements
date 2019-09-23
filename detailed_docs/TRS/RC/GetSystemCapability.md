## Functional Requirements


1.
In case  
mob app sends GetSystemCapability(systemCapabilityType="REMOTE_CONTROL") request

SDL must  
respond GetSystemCapability to the app
with `remoteControlCapabilities` received from HMI (via RC.GetCapabilities response)

_Note: HMI is supposed to includes all parameters in RC.GetCapabilities response even if they are defined as optional._ 


2.
In case  
mob app sends GetSystemCapability request  
and SDL received only mandatory parameters within all capabilities provided by HMI

SDL must

respond GetSystemCapability with all modules capabilities to the app including `allowMultipleAccess` set to `true`

3.
In case  
mob app sends GetSystemCapability request  
and SDL received from HMI module capabilities with omitted mandatory parameters or module capabilities with invalid values

SDL must

sends "GetSystemCapability" response with default module capabilities to the app

## Non-functional Requirements

### Mobile API

```
    <function name="GetSystemCapability" functionID="GetSystemCapabilityID" messagetype="request" since="4.5">
        <description>Request for expanded information about a supported system/HMI capability</description>
        <param name="systemCapabilityType" type="SystemCapabilityType" mandatory="true">
            <description>The type of system capability to get more information on</description>
        </param>
        <param name="subscribe" type="Boolean" mandatory="false" since="5.1">
            <description>Flag to subscribe to updates of the supplied service capability type. If true, the requester will be subscribed. If false, the requester will not be subscribed and be removed as a subscriber if it was previously subscribed.</description>
        </param>
    </function>
    
    <function name="GetSystemCapability" functionID="GetSystemCapabilityID" messagetype="response" since="4.5">
        <param name="systemCapability" type="SystemCapability" mandatory="true">
        </param>
        <param name="resultCode" type="Result" platform="documentation" mandatory="true">
            <description>See Result</description>
            <element name="SUCCESS"/>
            <element name="INVALID_DATA"/>
            <element name="OUT_OF_MEMORY"/>
            <element name="TOO_MANY_PENDING_REQUESTS"/>
            <element name="APPLICATION_NOT_REGISTERED"/>
            <element name="GENERIC_ERROR"/>
            <element name="REJECTED"/>
            <element name="IGNORED"/>
            <element name="DISALLOWED"/>
            <element name="USER_DISALLOWED"/>
            <element name="UNSUPPORTED_RESOURCE">
                <description>The capability does not exist on the module</description>
            </element>
            <element name="DATA_NOT_AVAILABLE">
                <description>The capability should exist on the module but there was an error retrieving the data.</description>
            </element>
        </param>
        <param name="info" type="String" maxlength="1000" mandatory="false">
            <description>Provides additional human readable info regarding the result.</description>
        </param>
        <param name="success" type="Boolean" platform="documentation" mandatory="true">
            <description> true if successful; false, if failed </description>
        </param>
    </function>
    ```

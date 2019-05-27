## Functional requirements

1.  	
In case
the application successfully registers to SDL

SDL must  
send OnPermissionsChange (`<assigned policies>`) to such application

2.
In case
the application is registered to SDL  
and the application's permissions are changed (items are either added or removed) because of applying the Updated Policy Table

SDL must  
send OnPermissionsChange (`<updated policies>`) to such application

_Note: if new "functional_grouping" was added in case of PTU -> SDL must still send this notification to app independently from user allows/disallows this grouping_

3.  
In case  
the application is registered to SDL  
and the application's permissions are changed because of applying user's consent settings

SDL must
send OnPermissionsChange (`<updated policies>`) to such application

4.  
On getting user consent 'YES' from HMI and storage into LocalPolicyTable  

SDL must  
notify an application about the current permissions active on HMI via OnPermissionsChange() notification

5.  
On getting user consent 'NO' from HMI and storage this data into LocalPolicyTable  

SDL must
1) assign only the permissions for which there is user consent in the local policy table and permissions (if any) that are listed in 'pre_DataConsent' section (that is, 'default_hmi', 'groups' and other) for this application 
2) notify HMI via OnAppPermissionChanged() notification about the applied permissions
3) notify mobile app via OnPermissionsChanged() notification about the applied permissions

## Non-Functional Requirements  
### Mobile API
1.

```
    <function name="OnPermissionsChange" functionID="OnPermissionsChangeID" messagetype="notification" since="2.0">
        <description>Provides update to app of which policy-table-enabled functions are available</description>
        <param name="permissionItem" type="PermissionItem" minsize="0" maxsize="500" array="true" mandatory="true">
            <description>Change in permissions for a given set of RPCs</description>
        </param>
    </function>
```

2.  
New Boolean `requireEncryption` param must be added to `OnPermissionsChange` RPC and to the `PermissionItem` data type.  
A list of `PermissionItem` will be received by the app via `OnPermissionsChange` notification, so that the app side knows whether the app needs encryption or not and which RPCs will need encryption.

_Note: to follow the naming conventions, `requireEncryption` is used in RPC messages and `encryption_required` in policy configuration. However, they shall refer to the same thing._ 

```xml
<function name="OnPermissionsChange" functionID="OnPermissionsChangeID" messagetype="notification" since="2.0">
    <description>Provides update to app of which policy-table-enabled functions are available</description>
    <param name="permissionItem" type="PermissionItem" minsize="0" maxsize="500" array="true" mandatory="true">
        <description>Change in permissions for a given set of RPCs</description>
    </param>
+   <param name="requireEncryption" type="Boolean"  mandatory="false" since="5.1"/>
</function>
```

```xml
<struct name="PermissionItem" since="2.0">
    <param name="rpcName" type="String" maxlength="100" mandatory="true">
        <description>Name of the individual RPC in the policy table.</description>
    </param>
    <param name="hmiPermissions" type="HMIPermissions"  mandatory="true"/>
    <param name="parameterPermissions" type="ParameterPermissions"  mandatory="true"/>
+   <param name="requireEncryption" type="Boolean"  mandatory="false" since="5.1"/>
</struct>
```

3.  
New result code "ENCRYPTION_NEEDED"  must be added to `Result` enum.

SDL must  
sends this code to a mobile app when it receives an un-encrypted PRC request message that needs encryption

```xml
<enum name="Result" internal_scope="base" since="1.0">
...
    <element name="ENCRYPTION_NEEDED" since="5.1">
        <description>SDL receives an un-encrypted PRC request that needs protection. </description>
    </element>
</enum>
```
---
# Create a Wi-Fi profile with a pre-shared key
Here's how to use Intune’s **Custom Configuration** to create a Wi-Fi profile with a pre-shared key. This topic also has an example of how to create an EAP-based Wi-Fi profile.

Note:
-	You may find it easier to copy the code from a computer that connects to that network, as described below.
- For Android, you also have the option of using this [Android PSK Generator](http://johnathonb.com/2015/05/intune-android-pre-shared-key-generator/) provided by Johnathon Biersack.
-	You can add multiple networks and keys by adding more OMA-URI Settings.
-  For iOS, use the Apple Configurator on a Mac station to configure the profile. Alternatively, use this [iOS PSK Mobile Config Generator](http://johnathonb.com/2015/05/intune-ios-psk-mobile-config-generator/) provided by Johnathon Biersack.


1.	To create a Wi-Fi profile with a pre-shared key for Android or Windows, or an EAP-based Wi-Fi profile, when you create a policy choose **Custom Configuration** for that device platform, rather than a Wi-Fi profile.

2.	Provide a name and description.
3.	Add a new OMA-URI setting:

   a.	Enter a name for this Wi-Fi network setting.

   b.	Enter a description of the OMA-URI setting or leave blank.

   c.	**Data Type**: Set to "String(XML)"

   d.	**OMA-URI**: ./Vendor/MSFT/Wi-Fi /Profile/<SSID>/Settings

Note: Be sure to include the dot character at the beginning.

SSID is the SSID for which you’re creating the policy. For example,
`./Vendor/MSFT/Wi-Fi/Profile/Hotspot-1/Settings`

  e.	Value Field: this is where you paste your XML code. Here’s an example. Each value should be adapted to your network settings. See the comments section of the code for some pointers.


    <!--
    <Name of wifi profile> = Name of profile
    <SSID of wifi profile> = Plain text of SSID. Does not need to be escaped, could be <name>Your Company's Network</name>
    <nonBroadcast><true/false></nonBroadcast>
    <Type of authentication> = Type of authentication used by the network, such as WPA2PSK.
    <Type of encryption> = Type of encryption used by the network
    <protected>false</protected> do not change this value, as true could cause device to expect an encrypted password and then try to decrypt it, which may result in a failed connection.
    <password> = Password to connect to the network
    -->
    <WLANProfile
    xmlns="http://www.microsoft.com/networking/WLAN/profile/v1">
      <name><Name of wifi profile></name>
      <SSIDConfig>
        <SSID>
          <hex>53534944</hex>
	    <name><SSID of wifi profile></name>
        </SSID>
        <nonBroadcast>false</nonBroadcast>
      </SSIDConfig>
      <connectionType>ESS</connectionType>
      <connectionMode>auto</connectionMode>
      <autoSwitch>false</autoSwitch>
      <MSM>
        <security>
          <authEncryption>
            <authentication><Type of authentication></authentication>
            <encryption><Type of encryption></encryption>
            <useOneX>false</useOneX>
          </authEncryption>
          <sharedKey>
            <keyType>networkKey</keyType>
            <protected>false</protected>
            <keyMaterial>MyPassword</keyMaterial>
          </sharedKey>
          <keyIndex>0</keyIndex>
        </security>
      </MSM>
    </WLANProfile>

## EAP-based Wi-Fi profile
Here’s  an example of the XML code for an EAP-based Wi-Fi profile:

    <WLANProfile xmlns="http://www.microsoft.com/networking/WLAN/profile/v1">
      <name>testcert</name>
      <SSIDConfig>
        <SSID>
          <hex>7465737463657274</hex>
          <name>testcert</name>
        </SSID>
        <nonBroadcast>true</nonBroadcast>
      </SSIDConfig>
      <connectionType>ESS</connectionType>
      <connectionMode>auto</connectionMode>
      <autoSwitch>false</autoSwitch>
      <MSM>
        <security>
          <authEncryption>
            <authentication>WPA2</authentication>
            <encryption>AES</encryption>
            <useOneX>true</useOneX>
            <FIPSMode     xmlns="http://www.microsoft.com/networking/WLAN/profile/v2">false</FIPSMode>
          </authEncryption>
          <PMKCacheMode>disabled</PMKCacheMode>
          <OneX xmlns="http://www.microsoft.com/networking/OneX/v1">
            <cacheUserData>false</cacheUserData>
            <authMode>user</authMode>
            <EAPConfig>
              <EapHostConfig     xmlns="http://www.microsoft.com/provisioning/EapHostConfig">
                <EapMethod>
                  <Type xmlns="http://www.microsoft.com/provisioning/EapCommon">13</Type>
                  <VendorId xmlns="http://www.microsoft.com/provisioning/EapCommon">0</VendorId>
                  <VendorType xmlns="http://www.microsoft.com/provisioning/EapCommon">0</VendorType>
                  <AuthorId xmlns="http://www.microsoft.com/provisioning/EapCommon">0</AuthorId>
                </EapMethod>
                <Config xmlns="http://www.microsoft.com/provisioning/EapHostConfig">
                  <Eap xmlns="http://www.microsoft.com/provisioning/BaseEapConnectionPropertiesV1">
                    <Type>13</Type>
                    <EapType xmlns="http://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV1">
                      <CredentialsSource>
                        <CertificateStore>
                          <SimpleCertSelection>true</SimpleCertSelection>
                        </CertificateStore>
                      </CredentialsSource>
                      <ServerValidation>
                        <DisableUserPromptForServerValidation>false</DisableUserPromptForServerValidation>
                        <ServerNames></ServerNames>
                      </ServerValidation>
                      <DifferentUsername>false</DifferentUsername>
                      <PerformServerValidation xmlns="http://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV2">false</PerformServerValidation>
                      <AcceptServerName xmlns="http://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV2">false</AcceptServerName>
                      <TLSExtensions xmlns="http://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV2">
                        <FilteringInfo xmlns="http://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV3">
                          <AllPurposeEnabled>true</AllPurposeEnabled>
                          <CAHashList Enabled="true">
                            <IssuerHash>75 f5 06 9c a4 12 0e 9b db bc a1 d9 9d d0 f0 75 fa 3b b8 78 </IssuerHash>
                          </CAHashList>
                          <EKUMapping>
                            <EKUMap>
                              <EKUName>Client Authentication</EKUName>
                              <EKUOID>1.3.6.1.5.5.7.3.2</EKUOID>
                            </EKUMap>
                          </EKUMapping>
                          <ClientAuthEKUList Enabled="true"/>
                          <AnyPurposeEKUList Enabled="false">
                            <EKUMapInList>
                              <EKUName>Client Authentication</EKUName>
                            </EKUMapInList>
                          </AnyPurposeEKUList>
                        </FilteringInfo>
                      </TLSExtensions>
                    </EapType>
                  </Eap>
                </Config>
              </EapHostConfig>
            </EAPConfig>
          </OneX>
        </security>
      </MSM>
    </WLANProfile>

4.	Click OK, then save and deploy the policy.
NOTE. This policy can only be deployed to user groups

The next time each device checks in, the policy will be applied, and a Wi-Fi profile will be created on the device. The device will be able to connect to the network automatically.
## Create the XML file from an existing Wi-Fi connection
You can also create an XML file from an existing Wi-Fi connection:
1.     On a computer that is connected to or has recently connected to the wireless network, open the following folder: C:\ProgramData\Microsoft\Wlansvc\Profiles\Interfaces\{guid}. It’s best to use a computer that has not connected to many wireless networks, as you’ll have to search through each profile to find the right one.
3.     Search through the XML files to locate the one with the right name.
4.     Once you have located the correct XML file, copy and paste the XML code into the Data field of the OMA-URI settings page.

### See also
[Help users connect to company resources using Wi-Fi profiles in Microsoft Intune](../Topic/help-users-connect-to-company-networks-using-wi-fi-profiles-with-microsoft-intune.md)

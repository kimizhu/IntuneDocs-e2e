---
title: Prepare iOS apps for mobile application management with the Microsoft Intune App Wrapping Tool
ms.custom: na
ms.reviewer: na
ms.service: microsoft-intune
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 99ab0369-5115-4dc8-83ea-db7239b0de97
author: Staciebarker
---
# Prepare iOS apps for mobile application management with the Microsoft Intune App Wrapping Tool
Use the **Microsoft Intune App Wrapping Tool for iOS** to modify the behavior of in-house iOS apps by restricting features of the app without changing the code of the app itself.

The tool is a Mac OS command line application that creates a ‘wrapper’ around an app. Once an app is processed, you can then change the apps functionality using an  [!INCLUDE[wit_nextref](./includes/wit_nextref_md.md)] mobile application management policy that you configure (see [Configure and deploy mobile application management policies in the Microsoft Intune console](configure-and-deploy-mobile-application-management-policies-in-the-microsoft-intune-console.md)).

To download the tool, see [Microsoft Intune App Wrapping Tool for iOS](http://www.microsoft.com/en-us/download/details.aspx?id=45218).

## <a name="BKMK_Prereq"></a>Step 1: Fulfill the prerequisites for using the app wrapping tool

|Requirement|Details and more information|
|---------------|--------------------------------|
|Supported operating system and toolset|You must run the app wrapping tool on a Mac computer that runs OS X 10.8.5 or later, which has the XCode toolset version 5 or later installed.|
|Signing certificate and provisioning profile|You must have an Apple signing certificate and provisioning profile. See your [Apple developer documentation](https://developer.apple.com/).|
|Processing an app with the App Wrapping Tool|Apps must be developed and signed by your company, or an independent software vendor (ISV). You cannot use this tool to process apps from the Apple Store. Apps must be written for iOS 7.0 or later. Apps must also be in the Position Independent Executable (PIE) format. For more information about the PIE format, see your Apple developer documentation. Lastly, the app must have the extension **.app**, or **.ipa** format.|
|Apps the wrapping tool cannot process|Encrypted apps, unsigned apps, and apps with extended file attributes.|
|Apps that use the Azure Active Directory Library (ADAL)|If your app uses ADAL, the app must incorporate an ADAL version greater than or equal to 1.0.2, and the developer must grant their app access to the Intune Mobile Application Management resource.<br /><br />See [Information for apps that use the Azure Active Directory Library (ADAL)](prepare-ios-apps-for-mobile-application-management-with-the-microsoft-intune-app-wrapping-tool.md#BKMK_ADAL) in this article for details about how to use ADAL.|
|Setting entitlements for your app|You must set entitlements, which give the app additional permissions and capabilities beyond those typically granted, before you wrap the app. See [Setting app entitlements](#BKMK_ios_entitlements) for instructions.|

## Step 2: Install the app wrapping tool

1.  From the **Microsoft Intune App Wrapping Tool for iOS** page on the [Microsoft Download Center](http://www.microsoft.com/en-us/download/default.aspx), download the installation file for the app wrapping tool to a Mac computer.

2.  On the Mac computer, double-click the installation file **Microsoft Intune App Wrapping Tool for iOS.dmg**.

3.  Click **Agree** to accept the End User License Agreement (EULA). The installer is mounted and displayed on the Mac computer.

4.  Open the installer and  copy the displayed files to a new folder on the Mac computer. You can now disconnect the mounted installer drive.

    You are now ready to run the app wrapping tool.

## Step 3: Run the app wrapping tool

1.  On the Mac computer, open a Terminal window and navigate to the folder where you saved the files. Because the executable resides inside the package, you’ll need to run the command as follows:

    ```
    ./IntuneMAMPackager.app/Contents/MacOS/IntuneMAMPackager –i /<path of input app>/<app filename> -o /<path to output folder>/<app filename> –p /<path to provisioning profile> –c <SHA1 hash of the certificate> -a <client ID of input app> -r <reply URI of input app> -v true
    ```
    > [!NOTE]
    > Some parameters are optional as shown in the table below.

    **Example:** The following example command runs the app wrapping tool on an app named **MyApp.ipa**. A provisioning profile and SHA-1 hash are specified. The processed app is created and stored in the **/users/myadmin/Documents** on the Mac computer.

    ```
    /users/myadmin/Downloads/IntuneMAMPackager.app/Contents/MacOS/IntuneMAMPackager -i /users/myadmin/Downloads/MyApp.ipa -o /users/myadmin/Documents/MyApp_Wrapped.ipa -p /users/myadmin/Downloads/My_Provisioning_Profile_.mobileprovision -c 12A3BC45D67EF8901A2B3CDEF4ABC5D6E7890FAB –a 20e1cd0d-268e-4308-9583-02ae97dd353e –r https://contoso/ -v true
    ```
    You can use the following command line properties with the app wrapping tool:

    |Property|More information|
    |------------|--------------------|
    |**-h**|Displays the available command-line properties for the app wrapping tool.|
    |**-i**|Specifies the path and file name of the input app.|
    |**-o**|Specifies the path in which to save the processed app.|
    |**-p**|Specifies the path to your provisioning profile for iOS apps.|
    |**-c**|Specifies the SHA1 hash of the signing certificate.|
    |**-a**|Client ID of the input app (in GUID format) if the app uses Azure Active Directory Libraries (Optional).|
    |**-r**|Redirect URI of the input app if the app uses Azure Active Directory Libraries (Optional).|
    |**-v**|Output verbose messages to the console (Optional).|

2.  After processing completes, the message **The application was successfully wrapped** will be displayed.

    If an error occurs, see [Error messages](prepare-ios-apps-for-mobile-application-management-with-the-microsoft-intune-app-wrapping-tool.md#BKMK_Error) for help.

3.  The wrapped app is saved in the output folder you specified previously. You can now upload the app into [!INCLUDE[wit_nextref](./includes/wit_nextref_md.md)] and associate it with a mobile application management policy.

    > [!IMPORTANT]
    > You must upload the app as a new app. You cannot update an older, unwrapped version of the app.

    You can now deploy the app to your [!INCLUDE[wit_nextref](./includes/wit_nextref_md.md)] groups, and the app will now run on the device using the app restrictions you specify.

## Error messages and log files
Use the following information to troubleshoot issues you have with the app wrapping tool.

### <a name="BKMK_Error"></a>Error messages
If the app wrapping tool fails to complete successfully, one of the following error messages will be displayed:

|Error message|More information|
|-----------------|--------------------|
|You must specify a valid iOS provisioning profile.|Your provisioning profile might not be valid. Check to make sure you have the correct permissions for devices and that your profile is correctly targeting either development or distribution. Your provisioning profile might also be expired.|
|Specify a valid input application name.|Make sure that the input application name you specified is correct.|
|Specify a valid path to the output application.|Make sure that the path to the output application you specified exists, and is correct.|
|Specify a valid input provisioning profile.|Make sure you supplied a valid provisioning profile name and extension. Your provisioning profile might be missing entitlements or you might not have included the **–p** command-line option.|
|The input application you specified was not found. Specify a valid input application name and path.|Make sure your input app path is valid and exists. Make sure the input app exists at that location.|
|The input provisioning profile file you specified was not found. Specify a valid input provisioning profile file.|Make sure that the path to the input provisioning file is valid and that the file you specified exists.|
|The output application folder you specified was not found. Specify a valid path to the output application.|Make sure that the output path you specified is valid and exists.|
|Output app does not have .ipa extension.|Only apps with the **.app** and **.ipa** extensions are accepted by the app wrapping tool. Make sure your output file has a valid extension.|
|An invalid signing certificate was specified. Specify a valid Apple signing certificate.|Make sure you’ve downloaded the correct signing certificate from the Apple developer portal. Your certificate might also be expired. If your Apple certificate and provisioning profile can be used to correctly sign an app within Xcode, then they are valid for the app wrapping tool.|
|The input application you specified is invalid. Specify a valid application.|Make sure you have a valid iOS application that has been compiled as an .app or .ipa file.|
|The input application you specified is encrypted. Specify a valid unencrypted application.|The app wrapping tool does not support encrypted apps. Specify an unencrypted app.|
|The input application you specified is not in a Position Independent Executable (PIE) format. Specify a valid application in PIE format.|Position Independent Executable (PIE) apps can be loaded at a random memory address when run which can have security benefits. For more information, see your Apple Developer documentation.|
|The input app you specified has already been wrapped. Specify a valid unwrapped application.|You cannot process an app that has already been processed by the tool. If you want to process an app again, run the tool using the original version of the app.|
|The input application you specified is not signed. Specify a valid signed application.|The app wrapping tool requires apps to be signed. Consult your developer documentation to learn how to sign a wrapped app.|
|The input application you specified must be in the .ipa or .app format.|Only .app and .ipa extensions are accepted by the app wrapping tool. Make sure your input file has a valid extension and has been compiled as a .app or .ipa file.|
|The input app you specified has already been wrapped and is on the latest policy template version.|The app wrapping tool will not rewrap an existing wrapped app with the latest policy template version.|
|The given Azure Active Directory Client ID not a well-formed GUID. Please specify a valid Client ID.|When using the Client ID parameter, make sure you’ve provided a valid Client ID in GUID format.|
|The given Azure Active Directory reply URI is not a well-formed URI. Please specify a valid reply URI.|When using the reply URI command line property, make sure you’ve provided a valid reply URI.|
|WARNING: You did not specify a SHA1 certificate hash. Make sure that your wrapped application is signed before deploying.|Ensure that you specify a valid SHA hash (using the **–c** command line property).|

### Log files for the app wrapping tool
Apps that have been wrapped by using the app wrapping tool generate logs which are written to the iOS client device console. This information is useful in situations where you are having problems with the application and need to diagnose if the issue is related to the app wrapping tool. To retrieve this information, use the following steps:

1.  Reproduce the issue by running the app.

2.  Collect the console output by following Apple's instructions for [Debugging Deployed iOS Apps](https://developer.apple.com/library/ios/qa/qa1747/_index.html).

3.  Filter the saved logs for App Restrictions output by entering the following script into the console:

    ```
    grep “IntuneAppRestrictions” <text file containing console output> > <required filtered log file name>
    ```
    You can submit the filtered logs to Microsoft.

    > [!NOTE]
    > In the log file, the item ‘build version’ represents the build version of Xcode.

    Wrapped apps will also present users the option to send logs directly from the device via email after the app crashes. Users can send the log to you to examine and forward to Microsoft if required.

### <a name="BKMK_ADAL"></a>Information for apps that use the Azure Active Directory Library (ADAL)
The information in this section applies only to apps that use the Azure Active Directory Library (ADAL). If you are unsure if your app uses this library, contact the developer of the app.

The app must incorporate an ADAL version greater than or equal to 1.0.2.

For apps that use ADAL, the following must be true:

-   The app must incorporate an ADAL version greater than or equal to 1.0.2

-   Developers must grant their app access to the Intune Mobile Application Management resource, as described in [Steps to follow for apps that use ADAL](#BKMK_step_use_adal).

#### <a name="BKMK_adal_overview"></a>Overview of identifiers you need to get
Apps that use ADAL must register via the Azure management portal to obtain two unique identifiers for their app:

|Identifier|More information|Default value|
|--------------|--------------------|-----------------|
|**Client ID**|A unique GUID identifier is generated for each app after it registers with Azure Active Directory.<br /><br />If you know the app's specific client ID, you can specify this value. Otherwise, the default value must be used.|6c7e8096-f593-4d72-807f-a5f86dcc9c77|
|**Redirect URI**|A URI value that developers can provide when registering their app with Azure Active Directory to ensure that authentication tokens are returned specifically to that endpoint.<br /><br />Supplying a redirect URI is an optional parameter for the app wrapping tool. If it is not specified, a default URI will be used.|urn:ietf:wg:oauth:2.0:oob|
When you do not supply the above values to the tool; the following additional values are automatically used:

-   **Authority URI** - https://login.windows.net/common/oauth2/authorize

-   **Resource ID** - https://intunemam.microsoft.com

#### <a name="BKMK_step_use_adal"></a>Steps to follow for apps that use ADAL

1.  Review [Overview of identifiers you need to get](#BKMK_adal_overview) to identify the values that you need to get.

2.  Configure access to mobile application management in Azure Active Directory by doing the following:

    1.  Log into an existing Azure Active Directory account in the Azure management portal.

    2.  Click **existing LOB application registration** in Azure Active Directory.

    3.  In the configure section, choose **Configure Access to Web APIs in other applications**.

    4.  In the **Permission to other applications** section, from the first drop-down list, choose **Intune Mobile Application Management**.

        You can now use the app's Client ID in the app wrapping tool. You can find the app's Client ID in the Azure Active Directory management portal as described in the [Overview of identifiers you need to get](#BKMK_adal_overview) section.

3.  Use the **Client-ID** (using the property **–a**) and **Redirect-URI** values as command-line properties in the app wrapping tool. If you do not have these values, the default values are used. You must specify both values, or the end user will not be able to successfully authenticate the processed app.

#### Certificate, provisioning profile, and authentication requirements

|Requirement|Details|
|---------------|-----------|
|Provisioning profile|**Make sure that the provisioning profile is valid before you include it** - The app wrapping tool does not check whether the provisioning profile is expired when processing an iOS app. If an expired provisioning profile is specified, the app wrapping tool will include the expired provisioning profile, and you will not know there is a problem until the app fails to install on an iOS device.|
|Certificate|**Make sure that the certificate is valid before you specify it** - The tool does not check whether a certificate is expired when processing iOS apps. If the hash for an expired certificate is provided, the tool will process and sign the app, but it will fail to install on devices.<br /><br />**Make sure that the certificate provided for signing the packaged application has a match in the provisioning profile** - The tool does not validate if the provisioning profile has a match for the certificate provided for signing the wrapped application.|
|Authentication|A device must have a pin set for encryption to work. On devices to which you have deployed a wrapped application, touching the status bar on the device will require the user to re-authenticate with [!INCLUDE[wit_nextref](./includes/wit_nextref_md.md)]. The default policy in a wrapped application is *authentication on re-launch*. iOS handles any external notification (for example a phone call) as exiting the app and then re-launching it.<br /><br />For wrapped apps, the first user who signs into any wrapped app from the same publisher is cached. After this point, only that user is allowed to access the app. To reset the user, the device has to be unenrolled and then re-enrolled.|

#### Troubleshooting and technical notes about ADAP

-   If no ADAL resources are found, the tool will include the ADAL dynamic library. The tool will search for the ADAL library with a name of **ADALiOS.bundle** in the root folder.

-   The tool does not search for ADAL binaries (if any) within the app. If the app links to an outdated version, there may be runtime errors during login if authentication policies are enabled.

-   [!INCLUDE[wit_nextref](./includes/wit_nextref_md.md)] fetches the AAD token to the [!INCLUDE[wit_nextref](./includes/wit_nextref_md.md)] MAM resource-id for authentication. However, it is not used in any call that would in-turn verify the validity of the token. [!INCLUDE[wit_nextref](./includes/wit_nextref_md.md)] only reads the UPN of the signed-in user to determine app access. The AAD token is not used for any further service calls.

-   Authentication tokens are shared between apps from the same publisher since they are stored in shared keychain. If you want to isolate a specific app, then you must use a different signing certificate and provisioning profile for that app.

-   Double login prompts are prevented if you provide your client application’s Client ID and Redirect URI. This Client ID needs to be registered to access the published [!INCLUDE[wit_nextref](./includes/wit_nextref_md.md)] MAM resource ID in the AAD Dashboard. Failure to do so will result in a login failure when the app runs.

## <a name="BKMK_ios_entitlements"></a>Setting app entitlements
Before wrapping your app, you can grant **entitlements** to give the app  additional permissions and capabilities that exceed what an app typically can do.   An **entitlement file** is used during code signing to specify special permissions within your app (for example, access to a shared keychain). Specific app services, called **capabilities**, are enabled within Xcode during app development. Once enabled, the capabilities are reflected in your entitlements file. For more information about entitlements and capabilities, see [Adding Capabilities](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html) in the iOS Developer Library. For a complete list of supported capabilities, see [Supported capabilities](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/SupportedCapabilities/SupportedCapabilities.html).

### Supported capabilities for the App Wrapping Tool for iOS

|Capability|Description|Recommended guidance|
|--------------|---------------|------------------------|
|App Groups|Use app groups to allow multiple apps to access shared containers and allow additional inter-process communication between apps.<br /><br />To enable app groups, open the **Capabilities** pane and click the **ON** switch in the **App Groups** section. You can add app groups or select existing ones.|When using App Groups, use reverse DNS notation:<br /><br />*group.com.companyName.AppGroup*|
|Background Modes|Enabling background modes allows your iOS app to continue running in the background.||
|Data Protection|Data protection adds a level of security to files stored on disk by your iOS app. Data protection uses the built-in encryption hardware present on specific devices to store files in an encrypted format on disk. Your app needs to be provisioned to use data protection.||
|In-App Purchase|In-App purchase embeds a store directly into your app by enabling you to connect to the store and securely process payments from the user. You can use In-App Purchase to collect payment for enhanced functionality or for additional content usable by your app.||
|Keychain Sharing|Enabling keychain sharing allows your app to share passwords in the keychain with other apps developed by your team.|When using Keychain Sharing, use reverse DNS notation:<br /><br />*com.companyName.KeychainGroup*|
|Personal VPN|Enable personal VPN to allow your app to create and control a custom system VPN configuration using the Network Extension framework.||
|Push Notifications|Apple Push Notification service (APNs) allows an app that isn’t running in the foreground to notify the user that it has information for the user.|For push notifications to work, you need to use an app-specific provisioning profile.<br /><br />Follow the steps in the [Apple developer documentation](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html).|
|Wireless Accessory Configuration|Enabling wireless accessory configuration adds the External Accessory framework to your project and allows your app to configure MFi Wi-Fi accessories.||

### Steps to enable entitlements

1.  Enable capabilities in your app:

    1.  In Xcode, navigate to your app’s target, and click the **Capabilities** pane.

    2.  Turn on the appropriate capabilities. For detailed information about each capability and how to determine the correct values, see [Adding Capabilities](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/AddingCapabilities/AddingCapabilities.html) in the iOS Developer Library.

    3.  Note any IDs that you created during the process.

    4.  Build and sign your app to be wrapped.

2.  Enable entitlements in your provisioning profile:

    1.  Log in to the Apple Developer Member Center.

    2.  Create a provisioning profile for your app. For instructions, see [How to Obtain the Prerequisites for the Intune App Wrapping Tool for iOS](http://blogs.technet.com/b/microsoftintune/archive/2015/02/25/how-to-obtain-the-prerequisites-for-the-intune-app-wrapping-tool-for-ios.aspx).

    3.  In your provisioning profile, enable the same entitlements that you have in your app. You will need to supply the same IDs that you specified during the development of your app.

    4.  Complete the provisioning profile wizard and download your file.

3.  Ensure that you have satisfied all the prerequisites, and then wrap the app.

### Troubleshooting common errors with entitlements
If the App Wrapping Tool for iOS displays an entitlement error, try the following troubleshooting steps.

|Issue|Cause|Resolution|
|---------|---------|--------------|
|Failed to parse entitlements generated from the input application.|The App Wrapping Tool cannot read the entitlements file that was extracted from the app. The entitlements file might be malformed.|Inspect the entitlements file for your app. To do so, follow the instructions detailed below. When inspecting the entitlements file, check for any malformed syntax. The file should be in XML format.|
|Entitlements are missing in the provisioning profile (missing entitlements are listed). Repackage the app with a provisioning profile that has these entitlements.|There is a mismatch between the entitlements enabled in the provisioning profile and the capabilities enabled in the app. This mismatch also applies to the IDs associated with particular capabilities (i.e., App Groups, Keychain Access etc).|Generally, you can create a new provisioning profile that enables the same capabilities as the app. When IDs between the profile and app don't match, the App Wrapping Tool will replace the IDs if it is able to. If you still get this error after creating a new provisioning profile, you can try  removing entitlements from the app by using the **–e** parameter (see [Using the –e parameter to remove entitlements from an app](#BKMK_ios_use_e_param).|

#### Finding the existing entitlements of a signed app
To review the existing entitlements of a signed app and provisioning profile:

1.  Find the .ipa file and change its the extension to .zip.

2.  Expand the .zip file. This will produce a Payload folder containing your .app bundle.

3.  Use the codesign tool to check the entitlements on the .app bundle like this:

    ```
    $ codesign -d --entitlements :- "Payload/YourApp.app"
    ```
    where `YourApp.app` is the actual name of your .app bundle.

4.  Use the security tool to check the entitlements of the app's embedded provisioning profile:

    ```
    $ security -D -i "Payload/YourApp.app/embedded.mobileprovision"
    ```
    where `YourApp.app` is the actual name of your .app bundle.

#### <a name="BKMK_ios_use_e_param"></a>Using the –e parameter to remove entitlements from an app
This command removes any enabled capabilities in the app that are not in the entitlements file. If you remove capabilities that are being used by the app, it can break your app. An example of where you might remove missing capabilities is if you have a vendor-produced app that has all capabilities by default.

```
./IntuneMAMPackager.app/Contents/MacOS/IntuneMAMPackager –i /<path of input app>/<app filename> -o /<path to output folder>/<app filename> –p /<path to provisioning profile> –c <SHA1 hash of the certificate> -e
```

## Security and privacy for the app wrapping tool
Use the following security and privacy best practices when you use the app wrapping tool.

-   The signing certificate, provisioning profile and the line-of-business app you specify must be on the same Mac computer that you use to run the app wrapping tool. If the files are on a UNC path, ensure that these are accessible from the Mac computer. The path must be secured via IPsec or SMB signing.

    The wrapped application imported into the [!INCLUDE[wit_nextref](./includes/wit_nextref_md.md)] console should be on the same computer that you run the tool on. If the file is on a UNC path, ensure that it is accessible on the computer running the [!INCLUDE[wit_nextref](./includes/wit_nextref_md.md)] console. The path must be secured via IPsec or SMB signing.

-   The environment where the app wrapping tool is downloaded from the Microsoft Download Center site needs to be secured via IPsec or SMB signing.

-   The app you process must come from a trust-worthy source to ensure protection against attacks.

-   Ensure that the output folder you specify in the app wrapping tool is secured, particularly if it is a remote folder.

-   iOS apps that include a file upload dialog box can allow users to circumvent cut, copy and paste restrictions applied to the app. For example, a user could use the file upload dialog box to upload a screenshot of the app data.

-   When users monitor the documents folder on their device from within a wrapped app, they might see a folder named **.msftintuneapplauncher**. If this folder is changed or deleted, this might affect the correct functioning of restricted apps.

### See also
[Configure and deploy mobile application management policies in the Microsoft Intune console](configure-and-deploy-mobile-application-management-policies-in-the-microsoft-intune-console.md)</br>
[Decide how to prepare apps for mobile application management with Microsoft Intune](decide-how-to-prepare-apps-for-mobile-application-management-with-microsoft-intune.md)</br>
[Use the SDK to enable apps for mobile application management](use-the-sdk-to-enable-apps-for-mobile-application-management.md)



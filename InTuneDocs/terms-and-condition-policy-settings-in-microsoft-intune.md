---
title: Terms and condition policy settings in Microsoft Intune
ms.custom: na
ms.reviewer: na
ms.service: microsoft-intune
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6edf0ac1-4f46-4543-a9e5-f484ac37e9a5
author: NathBarn
---
# Terms and condition policy settings in Microsoft Intune
You can deploy [!INCLUDE[wit_nextref](./includes/wit_nextref_md.md)] terms and conditions to user groups to explain how enrollment, access to work resources, and using the company portal affect devices and users. Users must accept the terms and conditions before they can use the company portal to enroll and access their work.

## Working with terms and conditions policies in Intune
You can create and deploy multiple policies containing different terms and conditions. You can also produce versions of the same terms and conditions in different languages and then deploy these to their appropriate groups.

#### To create a terms and conditions policy

1.  In the [Microsoft Intune administration console](http://manage.microsoft.com) click **Policy** &gt; **Terms and Conditions**.

2.  Click **Add** to create a new terms and conditions policy.

    You can also **Edit** or **Delete** an existing policy.

3.  On the **Create Terms and Conditions** page, specify the following information:

    -   **Name** - A unique policy name displayed in the Intune console

    -   **Description** - Details that help you identify the policy in the Intune console

    -   **Title** - The title displayed to users in the company portal

    -   **Text to explain what it means if the user accepts** - Label users see regarding acceptance. **Example**: “I agree to the terms and conditions.”

4.  When you are finished, click **Save**. The new policy is displayed in the **Terms and Conditions** node of the **Policy** workspace.

#### To deploy a terms and conditions policy

1.  In the [Microsoft Intune administration console](http://manage.microsoft.com) click **Policy** &gt; **Terms and Conditions**.

2.  In the **Terms and Conditions Policies** list, select the policy you want to deploy, and then click **Manage Deployment**.

3.  In the **Manage Deployment** dialog box, select the user groups who you want to deploy the policy to, and then click **OK**.

    When targeted users access the company portal, Intune displays the terms and conditions you deployed. Users must accept these terms before they can gain access to company resources.

#### To monitor a terms and conditions policy

1.  In the [Microsoft Intune administration console](http://manage.microsoft.com) click **Policy** &gt; **Terms and Conditions**.

2.  In the **Create New Report** window, click **View Report**. The report will open detailing which users have accepted the terms and conditions you deployed.

### <a name="BKMK_TCVers"></a>Updates and version control for terms and conditions
When you edit an existing terms and conditions policy, you can choose the behavior when you deploy the policy. Use the following procedure to help you update existing terms and conditions policies.

##### How to work with multiple versions of terms and conditions

1.  In the [Microsoft Intune administration console](http://manage.microsoft.com) click **Policy** &gt; **Terms and Conditions**.

2.  Select the terms and conditions policy that you want to edit, and then click **Edit**.

3.  On the **Edit Terms and Conditions** page, make any required edits, and then specify whether this new version requires all users to accept the terms and conditions, or only new users will see the new version.

    We recommend you increase the version number and require acceptance any time you make significant changes to your terms and conditions policy. Keep the current version number if you are fixing typos or changing formatting, for example.

## See Also
[Use policies to manage computers and mobile devices with Microsoft Intune](use-policies-to-manage-computers-and-mobile-devices-with-microsoft-intune.md)


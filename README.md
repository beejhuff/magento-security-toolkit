# The Magento Security Toolkit (MST) #

[![Join the chat at https://gitter.im/comitdevelopers/magento-security-toolkit](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/comitdevelopers/magento-security-toolkit?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

Website: http://comitdevelopers.github.io/magento-security-toolkit/

 **ALERT - On 08-04-2015 & 07-07-2015** *On July 07, 2015 Magento officially released several important Security announcements affecting the Magentio Community. Please take note of the following security-related announcements before using the Magento Security Toolkit*

 1. **The Magento Security Center** is now [open to the entire to the Magento Community](http://magento.com/security) - You can now register here for priority email alerts on Magento Security Updates.  We encourage everyone to register now to recieve timely updates on Magento Security Topics as soon as they are made available by Magento.  You can also revdiew Magento's Responsible Disclosure Guidelines, learn how to participate in their Bug Bounty Program and how to use their Cryptigraphic Key to Sign and Encrypt any sensitive communications you need to exchange with Magento.  
 
 2. **SUPEE-6285** ([Release Notes](http://merch.docs.magento.com/ce/user_guide/Magento_Community_Edition_User_Guide.html#magento/patch-releases-2015.html)) - This patch addresses 2 CRITICAL, 5 MEDIUM, and 2 LOW severity Security Vulnerabilities.  It's included in the Official Magento CE 1.9.2 release, but if you need to ensure that your systems's security issues are addressed in a timely mannger, you may wish to test and install the patch alone.  It ONLY inlcudes updates related to the Security Vulnerabilities disclosed, which the full release includes many other features that may reuire additional testing and validation to ensure compatibility with an existing Magento deployment.
  
 3. **Magento CE 1.9.2** ([Release Notes](http://merch.docs.magento.com/ce/user_guide/Magento_Community_Edition_User_Guide.html#magento/release-notes-ce-1.9.2.html))

 4. **Updated Magento CE [Downloads Section](http://merch.docs.magento.com/ce/user_guide/Magento_Community_Edition_User_Guide.html#magento/patch-releases-2015.html)** The new Magento CE Downloads section has been reorganzed to make it REALLY easy to find the most recent full Version of Magento CE at the very top of the page while being able to scroll down the page to see all of the patches that are also available.  You can now use a simple drop down to verify which versions of CE each patch is designed for and quickly download them for installation.  (SEE BELOW)


There are still some very slight discrepancies in the UI (APPSEC-212 for example lists the CE Versions in the opposite order of all the other lists, SUPEE-2725 appears to leave a gap indicating that CE 1.6.2 isn't affected and a few of the remaining patches use a slightly different Patch naming schema based on their original composition for Magento E


Our current research on known attack signatures has been focused on SUPEE-5344 and SUPEE-5994 is [briefly covered in the Markdown summary](/signatures-analysis.md) - please add whatever else you've discovered yourself and submit a pull request, we're ready to merge!  If you'd prefer to submit anything directly or anonymously, you can also use any of the methods below to contact us.

* Email : signatures@comitdevelopers.com

* Discussion Group : 

    * (via email list) magento-security-toolkit@comitdevelopers.com 

    * (via Google Groups) https://groups.google.com/a/comitdevelopers.com/d/forum/magento-security-toolkit

* Gitter Chat Room : https://gitter.im/comitdevelopers/magento-security-toolkit

----------

## What is The Magento Security Toolkit?

Our goal is to fill the gap between the understandably limited amount of technical detail published on security vulnerabilities that are responsibly disclosed by Researchers and the information supplied by Magento in their communications and associated software patches designed to address those vulnerabilities.  

We understand why it is necessary for all parties to behave in a deliberate and responsible manner when addressing concerns affecting the security of software that is so critically important to the livelihoods of millions of Merchants and their families around the world.  We are developing the Magento Security Toolkit to provide every member of the Magneto Community a set of tools and the knowledge required to effectively combat the security threats that target Magento.

We accomplish this through 2 main activities:

1. Developing a set of n98-magerun add-ons that can that do more than just check to see if you have applied a set of patches or that you have a specific version of Magento installed.  You will be able to use the MST to analyze your Magento codebase in the context of any malware, attack signatures, and known compromises that have been shared  by fellow professionals Magento Community.

2.  Providing a forum in which all Members of the Magneto Community are made welcome to share their knowledge from researching and addressing Magento security vulnerabilities.  We use the information in a responsible manner and we  use this repository to share the collective Wisdom earned through the difficult task of analyzing, researching, and responding to attacks against Magento software.

All content and code provided here is protected by the copyright or their respective owners and are provided under the [Creative Commons 4.0 Attribution International License](/comitdevelopers/magento-security-toolkit/blob/master/cc-4-international.md)  [an the open source MIT License](https://github.com/comitdevelopers/magento-security-toolkit/blob/master/LICENSE).

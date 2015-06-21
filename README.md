# The Magento Security Toolkit (MST) #

[![Join the chat at https://gitter.im/comitdevelopers/magento-security-toolkit](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/comitdevelopers/magento-security-toolkit?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

***IMPORTANT NOTE*** Our current research on known attack signatures related to SUPEE-5344 and SUPEE-5994 is [briefly summarised in the this PDF document](https://github.com/comitdevelopers/magento-security-toolkit/blob/master/Comit%20Developers-%20Analysis-%20Mitigation-%20and%20Remediation%20of%20Issues%20addressed%20by%20SUPEE-5344&%20SUPEE-5994.pdf) - please review it and let us know if you have identified any additional signatures which we have not yet documented.  You can submit signatures or any other feedback via

* Email : signatures@comitdevelopers.com
* Discussion Group : magento-security-toolkit@comitdevelopers.com (https://groups.google.com/a/comitdevelopers.com/d/forum/magento-security-toolkit)
* Gitter Chat Room : https://gitter.im/comitdevelopers/magento-security-toolkit

----------

## What is The Magento Security Toolkit?

The MST is designed to fill the gap between the understandably limited amount of technical detail published on security vulnerabilities that are responsibly disclosed by Security Researchers and the information supplied by Magento in their communications and associated software patches designed to address those vulnerabilities.  We realize why it is necessary for all parties to behave in a deliberate and responsible manner when addressing concerns affecting the security of Magento.  We have developed the Magento Security Toolkit to provide every member of the Magneto Community a set of tools and a platform for sharing the knowledge and solutions required to effectively combat the security threats that target Magento.

We accomplish this through three areas of focused and dedicated effort:

1. We develop, distrbute and maintain a suite of testing tools that do more than just check to see if you have applied a set of patches or that you are running a specific release version of Magento.  The MST analyzes the changes  made to the your Magento codebase in the context of the presence (or lack thereof) of any malware or exploit attack signatures and known  compromises.  The Magento Security Toolkit analyzes the specific permutations and combinations of signatures present in relation to your particular customizations to Magneto to identify the likely targets and possible ares of vulnerability.

2.  We provide a forum in which all Members of the Magneto Community are made welcome to share knowledge acquired while researching and addressing Magento security vulnerabilities.  We use the information provided to us in a responsible manner and we repay these contributions from the Magento Community by providing a freely available and glbally distributed Knowledge Base including Content we develop, curate, and publish online and by sharing our Knowlege and our Software to the world under [an the open source MIT License](https://github.com/comitdevelopers/magento-security-toolkit/blob/master/LICENSE).

3. We develop, distribute and maintain a collection of automated tools that remediate & repair compromised Magento systems from both detected attack signatures as well as restoring  compromised Magento, Vendor Plugin, and your own cusomteized Magento configurations and your own cusomer agento Appkication source files  (requires access to the network AND/ OR local repositories of Magento Code, Vendor Plugin code and your own sogtware repositories for analysis nad comparison).


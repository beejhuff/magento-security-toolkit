**Compromised Files, Attack Signatures and Validated Remediation Steps for the vulnerabilities addressed with the following patch installation files**

   * SUPEE-5344
   * SUPEE-5994

**External Symptoms (Modified Files & Attack Signatures) exhibited by Magento after partial or complete compromise**

>It is important to remember that the files you see in your own environment may NOT be altered in the exact same >manner as we describe below.  Most attacks are automated and in many cases the attackers will write programs that >slightly modify themselves over time as they attack more and more targets successfully so that they can more easily >escape detection after they have been in the wild for some time.  You may also find exact duplicates of the >behavior and files we highlight below.  If you are not well versed in troubleshooting and analyzing these kinds of >situations, you should contact a trained security professional before proceeding and ask for assistance to minimize >your risk of further damage and give yourself time to address the situation deliberately and thoroughly.


* **Signature:** Privileged users can’t Login into the Magento Admin Panel

 * Reference:[http://magento.stackexchange.com/questions/64461/error-logging-in-the-admin-panel-fatal-error-class-magpleasure-filesystem-help](http://magento.stackexchange.com/questions/64461/error-logging-in-the-admin-panel-fatal-error-class-magpleasure-filesystem-help)

 * This issue of having a blank screen prevent Admins from attempting to login to the Magento Backend was most frequently due to the attacker disabling all module output in Core Config table which prevented legitimate admin users from seeing errors that were being generated from the files the attackers modified. 

 * If you were able login to the Magento Admin, this probably hasn’t happened to your system yet.  On the most significantly affected site we examined, it appeared that the attackers didn't decide to trigger this attack until after they had control of the system for several hours.  If you can not login because you don’t see the form fields (just a white screen), then login to your database server via phpMyAdmin or a MySQL client and run the following query to confirm the presence of the signature ad to re-enable the Admin HTML Module Output.


```
# If a similar signature is present you might see a '1' in the value column of the result set
# that is returned from the following SQL Query

    SELECT * FROM db_amedadirect_ins.core_config_data
      WHERE path = 'advanced/modules_disable_output/Mage_Admin';

# The following query will reset this back to the default of '0' and then you can log back 
# into the Magento Admin after you reset your cache and modify any modules that should be 
# disabled back to their proper state.

    UPDATE db_amedadirect_ins.core_config_data
      SET value = 0
      WHERE path = 'advanced/modules_disable_output/Mage_Admin';

```

   * Reports identified other users seeing [similar behavior in Magento Admin and problems logging in....](http://magento.stackexchange.com/questions/64461/error-logging-in-the-admin-panel-fatal-error-class-magpleasure-filesystem-help)

* **Multiple Modified Files** in your application folders that create back doors into the system and provide mechanism to steal Customer, Merchant, and Credit Card Data during transit.  General references below, specific examples of identified signatures follow in subset section.

  * [http://magento.stackexchange.com/questions/67660/magento-hacked-even-after-applied-patch](http://magento.stackexchange.com/questions/67660/magento-hacked-even-after-applied-patch)
  * [http://invisiblezero.net/magento-secure-your-webshop/](http://invisiblezero.net/magento-secure-your-webshop/)
  * [https://blog.sucuri.net/2015/04/magento-shoplift-supee-5344-exploits-in-the-wild.html](https://blog.sucuri.net/2015/04/magento-shoplift-supee-5344-exploits-in-the-wild.html)
  * [http://devdocs.magento.com/guides/m1x/install/installer-privileges_after.html#extensions](http://devdocs.magento.com/guides/m1x/install/installer-privileges_after.html#extensions)
  * [https://blog.sucuri.net/2015/04/magento-shoplift-supee-5344-exploits-in-the-wild.html](https://blog.sucuri.net/2015/04/magento-shoplift-supee-5344-exploits-in-the-wild.html)

* ** Modified payment gateway transaction files allows attacker to intercept communications with the gateway merchant transaction process and send CC info to their own servers:** 
  * https://blog.sucuri.net/2015/04/impacts-of-a-hack-on-a-magento-ecommerce-website.html
  * http://magento.stackexchange.com/questions/67660/magento-hacked-even-after-applied-patch
 

* **Discover Magpleasure_Filesystem in app/code/Magpleaseure/Filesystem & app/etc/modules**
  * **File:** app/code/Magpleaseure/Filesystem & httpdocs/app/etc/modules/Magpleasure_Filesystem.xml

  * **Reference:** [https://www.reddit.com/r/hacking/comments/34025m/magento_exploit_downlaoding_magpleasurefilesystem/](https://www.reddit.com/r/hacking/comments/34025m/magento_exploit_downlaoding_magpleasurefilesystem/)

  * The following command will travers a Linux / Unix file system and print any matching files to the standard output. You may need to modify the beginning directory for your OS & webserver combination.
            
      ```$ find /var/www/ -type d -iname "Magpleasure”```

  * **File:** app/etc/modules/Magplesaure_Fileserver.xml (
  * Enables the filesystem module from Magpleasure previously demonstrated to be compromised.


* **A modified Version of the base Varien Object Class** replaces the original application source file.
  * **FILE:** httpdocs/lib/Varien/Object.php

  * When you first have a visitor hit your site AT a specific URI '/checkout|admin/‘ (which the attackers do 5 times in a row to trigger this hacked file to be executed vi the conditional statements added to the top of the file, this file.

  * This was commonly one of the first evidence of a compromised file on the attacked web server discovered.  It’s primary functions appeared to be twofold:

  * First, copy or create additional files stored in the var/media & var/cache subdirectories.  Provided additional opportunities to exploit weak file and folder permissions or unpatched or dated plugins that could be used to provide additional attack vectors if the original entry points were discovered and removed or disabled.

  * Second, to query the database for customer contact records and write the results of the queries to another set of files that are stored in the var/media var/cache directories.  These files were serialized and concatenated into Base 64 encoded streams and then written to a COOKIE variable that was passed back in an HTTP response for transmission to another host, presumably one controlled by the attacker.

* **You find that there has been an Installation of a Modified Version or changes the current file itself, but then deletes the code from itself that performs the modification)**

  * **File:** lib/Varien/Autoloader.php 

  * Throws Error: Mage PHP Notice: Undefined index: REQUEST_URI in /htdocs/lib/Varien/Autoload.php on line 1

  * Reference : [http://stackoverflow.com/questions/29939553/mage-php-notice-undefined-index-request-uri-in-htdocs-lib-varien-autoload-php](http://stackoverflow.com/questions/29939553/mage-php-notice-undefined-index-request-uri-in-htdocs-lib-varien-autoload-php)
 
* **Modified Version** of primary Magento application index file

  * **FILE:** httpdocs/index.php

  *   This is one of the interesting ones - this file is modified to invoke the Magento API calls to grab the configuration details for your Authorize.net , or other Merchant Provider login credentials as well as a dump of your Merchant Provider Transaction API Key out the system confirmation database tables.  Even if you are safety conscious and store this dat in an encrypted format, because the attackers don’t get the data directly from the database but rather use the Magento API to access it, they can rely on Magento to decrypt this data and pass it along to the calling function.

  *   The attacker prints these values to the screen to be saved by whatever bots they’re running to process the responses their code is sending via HTML.:  https://rackspeed.de/blog/bruteforce-attacken-und-hacks-von-magento-shops/


* **Compromise Credit Card Process Internal Components:**

  * **FILE:** app/code/core/Mage/Payment/Model/Method/Cc.php

  * This is file is modified to allow an attacker to route credit details over to another server for a man in the middle type attack agains user attempting to check out on the MAgento only earpieces.

  * **Reference:**
      * https://rackspeed.de/blog/bruteforce-attacken-und-hacks-von-magento-shops/
      * https://rackspeed.de/blog/bruteforce-attacken-und-hacks-von-magento-shops/
    
  * **FILE: ** /js/index.php **AND / OR** js/lib/ccard.js

  * **Modified source files** here to gain additional access to credit card processing credentials, account settings and information used to steal stored credit card information or access it during transit between wine host and merchant provider.

  *   Replaces CE Edition of /js/index.php oks replaced with a modified version of Magento EE js/index.php that ALSO 

  *   Examines the array of POST data elements fore a hash value

  *   If it finds that has an element of the array, it stores two other POST elements in two veraivbles in this PHP script that would normlly be used to output a javascript and css to the invoking browser and instead whites those files whose contents had just been replaced with the post data to disk in older to be executed on the server later in the attack.

  * **NOTE:** We did not personally capture nay evidence of the httpdocs/js/lin/cccard.js file being edified but given the number of times others have done so on line we felit prudent to include it intros analysis.

* **Magento Admin Panel Error Generated when you attempt to access it to login You are unable to login and get redirected over and over again in a loop to the login form .  The following message is logged to the var/app/log folder:** 

  * Error logging in the admin panel Fatal error: Class 'Magpleasure_Filesystem_Helper_Data' not found …/app/Mage.php on line 546

  * [http://magento.stackexchange.com/questions/64461/error-logging-in-the-admin-panel-fatal-error-class-magpleasure-filesystem-help](http://magento.stackexchange.com/questions/64461/error-logging-in-the-admin-panel-fatal-error-class-magpleasure-filesystem-help)

* **Discover of APC caching and / or Magento compilation are enabled on your system even though you did not enable it in PHP settings, configure APC in your original httpdocs/app/etc/local.xml or setup compilation in the Magento Admin console. **
  * During our investigations we determined that some Merchants suspected that they had been attacked in some way and potentially compromised.  Several searched the Internet and edified some of the articles and discussions threads reference in this document.  Finding suspsicious files on their system  they deleted them from the file system only to note that this had not apparent affect.  Pid closer examination and diff’s run against the source repos and the files system in place, we identified additional changes made to confirmations as well as parameters stored in the database that control directive like compilation. 


* **These changes to the environment allowed the attackers to avoid detection for an extended period while the Merchant and ourselves were chains down multi exam for additional time while securing access to additional privileged data form the Magento stores.**  

  * After diff’ing the deployed files against a few known good copy of the source code form the client’s git repository, we were able to identify not only the changed files but also the files hat had been added  and removed from the deployment directory of their Magento App.


* **Discover newly created Admin Users added to Magneto admin with Full Admin role assignment and associated permissions so that the attackers**

  * They could open up a browser an manually loving (or via a programmed bot) and extract additional details of historical transactions via MAgento’s built-in data export functions.  **The unauthorized administrative accounts were typically named something like:**  admin_hacj or admi_etqg

  * Accomplished either via an exploited SQL Injection Vulnerability not yet resolved by installing the available SUPEE-5994 updates or 

  * If any of the previous exploits provide access to the file system or shell environments due to privilege escalation or improperly secured file / directory permissions, by smily opening a shell and connecting to the database via the local mysql client.

  * We were able to confirm that both madhouse had been used but examining the local .mysql_history file on all sites and noting that on one of them, commands unrelated to code that was installed for the client had been executed during same window that we identified  other attacks being executed.

  * Capturing all of the log files as quickly as possible allowed us to test our hypothesis by correlating entries in the Apache, MySQL, Magento, Magento, OpenSSL SSlogs as well as the .mysql_history & .bash_history files in the users’s home directories in the user’s home directories when shell escalation was accomplished. 

      * **Reference :** [https://rackspeed.de/blog/bruteforce-attacken-und-hacks-von-magento-shops/](https://rackspeed.de/blog/bruteforce-attacken-und-hacks-von-magento-shops/)
      * **Reference:** [http://magentary.com/kb/magento-recovery-shoplift-vulnerability/](https://rackspeed.de/blog/bruteforce-attacken-und-hacks-von-magento-shops/)

    * Also, User ROLES are modified or created to grant additional privileges to  new users who are added during compromise

    * Will normally use the same domain name for email accounts but repeat the username portion for each added user so that on initial review the users appear to at least have the same email domain suffice to avoid detection.  closer examination reveals the attacker grabbed a sintle alreadyssociated with a user and used it for [beach of the additional accounts they created](https://rackspeed.de/blog/bruteforce-attacken-und-hacks-von-magento-shops/)

* **Related to Shoplift Attack:** 

  * **File:** httpdocs/get.php

  * [http://magento.stackexchange.com/questions/67660/magento-hacked-even-after-applied-patch](http://magento.stackexchange.com/questions/67660/magento-hacked-even-after-applied-patch)
  
  * **NOTE:** We did not identify any of these files as having been compromised on the sites we evaluated so we do not have a signature analysis;  If you have a compromised version of this file, please share it so we can add the signature to our database and improve scan accuracy.


* **MySQL Driver is altered to allow further attacker access to underlying database**

  * **File:** lib/Varien/Db/Adapter/Pdo/Mysql.php is modified, so patch can not be applied seamlessly

  * **Reference** [http://magentary.com/kb/magento-recovery-shoplift-vulnerability/](http://magentary.com/kb/magento-recovery-shoplift-vulnerability/)
  
  * **NOTE:** We did not identify any of these files as having been compromised on the sites we evaluated so we do not have a signature analysis;  If you have a compromised version of this file, please share it so we can add the signature to our database and improve scan accuracy.

* **New file that is already compromised is installed at media / dhl / info.php**
  
  * **Reference**  [http://community.magento.com/t5/Programming-Questions/Hacked-CE-installation-media-dlh-info-php/td-p/5428](http://community.magento.com/t5/Programming-Questions/Hacked-CE-installation-media-dlh-info-php/td-p/5428)
  
  * This file examines the post parameters passed when the file called through the URL and certain conditions are met it render the output of phpinfo() to the page provided the attacker with detailed information and setup of the current server to further provide an attack surface for executing additional compromises.
  
  * **Reference** [http://community.magento.com/t5/Programming-Questions/Hacked-CE-installation-media-dlh-info-php/td-p/5428](http://community.magento.com/t5/Programming-Questions/Hacked-CE-installation-media-dlh-info-php/td-p/5428)

* **Cookie Added allowing attackers to control and hijack user sessions and intercept the data that is associated with those sessions during chekcout**
  
  * **File:** app/code/core/Mage/Cms/controllers/IndexController.php_ file have a hijacking cookie key installed
  
  * [Reference](http://magentary.com/kb/magento-recovery-shoplift-vulnerability/)
  
  * **NOTE:** We did not identify any of these files as having been compromised on the sites we evaluated so we do not have a signature analysis;  If you have a compromised version of this file, please share it so we can add the signature to our database and improve scan accuracy.

* **MULTIPLE Files: Attackers place content in var/package/ folder that will be used later to attack site via Magpleasure/Filesystem module**   
  
  * **File:** File_System-1.0.0.xml -  contains references, hashes and payload information for files included in the Magpleasure_Fielsystem module that is compromised and is used to install the files for the compromised plugin in various locations on the system hard drive.

  * **NOTE:** The following file has suspicious payload and date / timestamps similar to he above file though we have been unable to confirm it as an attack vector with current analsysis.

  * **File:** tmp/package.xml : contains payloads file and directory naes as well as hashes that are coped to specific folders in the file system and expected to perofrn further attacks ataingt the system

  * **NOTE:** Dates of last file changes on all of these files are consistently the same day and the time stamps of the files are all VERY close to each other.  The attackers do not appear to be taking too strong of a precaution of hiding their tracks when it comes to adding files or modifying the date time stamp data to the compromised files.  Be when we diff the files reference in the package.xml they appear identical to the default Magento insulation folders.

  * The package.file contains the files, hash’s and file contents that get written to the hard drive via the MagePleasure file system extension.  This is the entry point for the compromise and if you don’t remove these then your site will be compromised again and again even if you patch it because the patch won’t prevent this functionality from being executed.

* **Additioinal Resources:**
  * [http://magentary.com/kb/magento-recovery-shoplift-vulnerability/](http://magentary.com/kb/magento-recovery-shoplift-vulnerability/)
  * [https://magento.stackexchange.com/questions/65097/fatal-error-class-magpleasure-filesystem-helper-data-not-found/65119#65119?newreg=7687cba75bc14bc6a590ee7421f456fb](https://magento.stackexchange.com/questions/65097/fatal-error-class-magpleasure-filesystem-helper-data-not-found/65119#65119?newreg=7687cba75bc14bc6a590ee7421f456fb)
  * [http://magento.stackexchange.com/questions/67660/magento-hacked-even-after-applied-patch](http://magento.stackexchange.com/questions/67660/magento-hacked-even-after-applied-patch)
  * [http://magento.stackexchange.com/questions/64930/applied-patches-but-still-getting-a-vulnerable-message?rq=1](http://magento.stackexchange.com/questions/64930/applied-patches-but-still-getting-a-vulnerable-message?rq=1)*
  * [http://invisiblezero.net/magento-secure-your-webshop/](http://invisiblezero.net/magento-secure-your-webshop/)
    

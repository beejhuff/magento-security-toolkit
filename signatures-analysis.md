
<div><br/></div>
<ul>
<li><span style="font-size: 18px;"><b>Compromised Files, Attack Signatures and Validated Remediation Steps</b></span> <b><span style="font-size: 18px;">for the</span></b> <span style="font-size: 18px;"><b>Magento</b></span> <b><span style="font-size: 18px;">vulnerabilities addressed with the following patch installation files</span><br/>
<br/></b>
<ul>
<li><b>SUPEE-5344</b></li>
<li><b>SUPEE-5994<br/>
<br/></b></li>
</ul>
</li>
<li><b><span style="font-size: 18px;">External Symptoms (Modified Files & Attack Signatures) exhibited by Magento after partial or complete compromise</span><br/>
<br/></b>
<ul>
<li><b>Can’t Login into the Magento Admin Panel:</b>
<ul>
<li>Reference: <a href="http://magento.stackexchange.com/questions/64461/error-logging-in-the-admin-panel-fatal-error-class-magpleasure-filesystem-help">http://magento.stackexchange.com/questions/64461/error-logging-in-the-admin-panel-fatal-error-class-magpleasure-filesystem-help</a></li>
<li>This is normally due to module output being disabled in the Magento Core Config Table to force a white screen to be displayed all over the site and prevent you from seeing errors that are being generated from the files the attackers modified. </li>
<li>I you can login to the Magento Admin, this has’t happened yet and you can move on.  If you can not login because you don’t see the form fields (just a white screen), then login to your database server via phpMyAdmin or a MySQL client and run the following query to confirm the presence of the signature ad to re-enable the Admin HTML Module OutputL</li>
</ul>
</li>
</ul>
</li>
</ul>
<div><br/></div>
<div style="margin-left:120px;"><font face="Courier New">-- If this signature is present, you will see a ‘1' in the 'value' column of th results set that is returned</font></div>
<div style="margin-left:120px;"><span style="font-family: 'Courier New';">SELECT * FROM db_amedadirect_ins.core_config_data</span></div>
<div style="margin-left:120px;"><span style="font-family: 'Courier New';">    WHERE path = 'advanced/modules_disable_output/Mage_Admin';</span></div>
<div style="margin-left:120px;"><span style="font-family: 'Courier New';"><br/></span></div>
<div style="margin-left:120px;"><span style="font-family: 'Courier New';"><br/></span></div>
<div style="margin-left:120px;"><span style="font-family: 'Courier New';">-- The following query will reset this back to the default of '0' and thenyou can log back into the Magento Admin after you reset your cache    </span></div>
<div style="margin-left:120px;"><span style="font-family: 'Courier New';">UPDATE db_amedadirect_ins.core_config_data</span></div>
<div style="margin-left:120px;"><span style="font-family: 'Courier New';">    SET value = 0</span></div>
<div style="margin-left:120px;"><span style="font-family: 'Courier New';">    WHERE path = 'advanced/modules_disable_output/Mage_Admin';</span></div>
<div><a href="http://magento.stackexchange.com/questions/64461/error-logging-in-the-admin-panel-fatal-error-class-magpleasure-filesystem-help"><br/></a></div>
<div><br/></div>
<ul>
<li style="display:inline;list-style:none;">
<ul>
<li><b>Multiple Modified Files in your source code directories that provide several “back doors" into the system and ways to steal Customer, Merchant, and Credit Card Data.  General references below, specific examples of identified signatures follow in subset section.</b>
<ul>
<li><a href="http://magento.stackexchange.com/questions/67660/magento-hacked-even-after-applied-patch">http://magento.stackexchange.com/questions/67660/magento-hacked-even-after-applied-patch</a></li>
<li><a href="http://invisiblezero.net/magento-secure-your-webshop/">http://invisiblezero.net/magento-secure-your-webshop/</a></li>
<li><a href="https://blog.sucuri.net/2015/04/magento-shoplift-supee-5344-exploits-in-the-wild.html">https://blog.sucuri.net/2015/04/magento-shoplift-supee-5344-exploits-in-the-wild.html</a></li>
<li><a href="http://devdocs.magento.com/guides/m1x/install/installer-privileges_after.html#extensions">http://devdocs.magento.com/guides/m1x/install/installer-privileges_after.html#extensions</a></li>
<li><a href="https://blog.sucuri.net/2015/04/magento-shoplift-supee-5344-exploits-in-the-wild.html">https://blog.sucuri.net/2015/04/magento-shoplift-supee-5344-exploits-in-the-wild.html<br/>
<br/>
<br/></a></li>
</ul>
</li>
<li><b>Stolen CC Specific modified file allows attacker to intercept communications with the gateway merchant transaction process and send CC info to their own servers:</b>
<ul>
<li>https://blog.sucuri.net/2015/04/impacts-of-a-hack-on-a-magento-ecommerce-website.html</li>
<li><a href="http://magento.stackexchange.com/questions/67660/magento-hacked-even-after-applied-patch">http://magento.stackexchange.com/questions/67660/magento-hacked-even-after-applied-patch<br/>
<br/>
<br/></a></li>
</ul>
</li>
<li><b>FILES:</b> <span style="font-family: 'Courier New';">app/code/Magpleaseure/Filesystem</span><b>  &</b> <span style="font-family: 'Courier New';">httpdocs/app/etc/modules/Magpleasure_Filesystem.xml</span> <b>: Discover Magpleasure_Filesystem in app/code/Magpleaseure/Filesystem & app/etc/modules</b>
<ul>
<li><a href="https://www.reddit.com/r/hacking/comments/34025m/magento_exploit_downlaoding_magpleasurefilesystem/">https://www.reddit.com/r/hacking/comments/34025m/magento_exploit_downlaoding_magpleasurefilesystem/<br/></a><span style="font-family: 'Courier New';"><br/>
<run this from a terminal on linux to find these files on your hard drive><br/></span><font face="Courier New"># find /var/www/ -type d -iname "Magpleasure”<br/>
<br/></font></li>
<li>You may also discover that there is a new file at<b> app/etc/modules/Magplesaure_Fileserver.xml</b> (or something names similarly)  that enables the module files you find above in your environment.  If you are unsure, check the file dates on the two files.  In all of our previous examples we found the time/date stamps were on the same day within  a few minutes of each other.<br/>
<span style="font-family: 'Courier New';"><br/>
<br/></span></li>
</ul>
</li>
<li><b>FILE: </b><span style="font-family: 'Courier New';">httpdocs/lib/Varien/Object.php</span> <b>: A new modified Version of lib/Varien/Object.php is added the source code folder for your site.  When you first have a visitor hit your site AT a specific URI '/checkout|admin/‘ (which the attackers do 5 times in a row to trigger this hacked file to be executed vi the conditional statements added to the top of the file, this file <br/>
<br/></b>
<ul>
<li>This was normally one of the first (if not the first) execution of  compromised file on the attacked we b server we would uncover.  It’s primary functions are twofold:
<ul>
<li>First, to create additional files on the server that are stored in the media/cache subdirectories.  These files allow additional exploits of the file system in case this exploit is discovered and removed.</li>
<li>Second, to Query the database for customer contact records and write the results of the queries to another set of files that are stored in the media/cache directories.  These files are serialized and concatenated into Base 64 encoded streams and then written to a COOKIE variable that is passed back in the HTTP response to either a console application or a listening agent that records the stream of customer data.  This will become important later in order to get the full value from the credit card merchant credentials that are stole since you can use this info to get other cards issued to the user based on the dimorphic data stolen.<br/>
<br/></li>
</ul>
</li>
<li>Throws Error: <span style="font-family: 'Courier New';">PHP Notice:  Undefined index: REQUEST_URI in httpdocs/lib/Varien/Object.php on line 6<br/>
<br/>
<br/></span></li>
</ul>
</li>
<li><b>FILE:</b> <span style="font-family: 'Courier New';">lib/Varien/Autoloader.php</span> <b>| Installs a Modified Version of lib/Varien/Autoloader.php (or changes the current file itslef, but then deletes the code from itself that performs the modification)</b>
<ul>
<li>Throws Error<span style="font-family: 'Courier New';">: Mage PHP Notice: Undefined index: REQUEST_URI in /htdocs/lib/Varien/Autoload.php on line 1</span></li>
<li>Reference : <a href="http://stackoverflow.com/questions/29939553/mage-php-notice-undefined-index-request-uri-in-htdocs-lib-varien-autoload-php">http://stackoverflow.com/questions/29939553/mage-php-notice-undefined-index-request-uri-in-htdocs-lib-varien-autoload-php<br/>
<br/>
<br/></a></li>
</ul>
</li>
<li><b>FILE:</b> <span style="font-family: 'Courier New';">httpdocs/index.php</span> <b>: Modified Version of /index.php</b>
<ul>
<li>This is one of the big ones - this file is modified to invoke the Magento API calls to grab the configuration details for your Authorize.net , or other Merchant Provider login credentials as well as a dump of your Merchant Provider Transaction API Key out the system confirmation database tables.  Even if you are safety concious and store this dat in an encrypted format, because the attackers don’t get the data directly from the database but rather use the Magento API to access it, they can rely on Magento to decrypt this data and pass it along to the calling function.</li>
<li>The attacker prints these values to the screen to be saved by whatever bots they’re running to process the responses their code is sending via HTML.<br/>
<br/></li>
</ul>
</li>
<li style="display:inline;list-style:none;">
<ul>
<li style="display:inline;list-style:none;">
<ul>
<li><a href="https://rackspeed.de/blog/bruteforce-attacken-und-hacks-von-magento-shops/"><br/></a></li>
</ul>
</li>
</ul>
</li>
<li><u><b>FILE:</b></u> <span style="font-family: 'Courier New';">app/code/core/Mage/Payment/Model/Method/Cc.php</span> : <u><b>Compromise Credit Card Process Internal Components:</b></u>
<ul>
<li>This is file is modified to allow an attacker to route credit details over to another server for a man in the middle type atttack agains user attempting to check out on the MAgento only earpieces.<br/></li>
<li><b>Reference</b>:  <a href="https://rackspeed.de/blog/bruteforce-attacken-und-hacks-von-magento-shops/">https://rackspeed.de/blog/bruteforce-attacken-und-hacks-von-magento-shops/<br/>
<br/>
<br/></a></li>
</ul>
</li>
<li><b>FILE: </b><span style="font-family: 'Courier New';">/js/index.php <b>AND / OR</b></span> <span style="font-family: 'Courier New';">js/lib/ccard.js</span> <b>: Modifed source feils here to gain additional access to credit card processing credentials, account settings and information used to steal stored credit card information or access it during transit between wine host and merchant provider.</b>
<ul>
<li>Replaces CE Editionof <span style="font-family: 'Courier New';">/js/index.php</span> oks replaced with a modified version of Magento EE js/index.php that ALSO 
<ul>
<li>Examines the array of POST data elements fore a hash value</li>
<li>If it finds that has an element of the array, it stores two other POST elements in two veraivbles in this PHP script that would normlly be used to output a javascript and css to the invoking browser and instead whites those files whose contents had just been replaced with the post data to disk in older to be executed on the server later in the attack.<br/>
<br/>
NOTE: We did not personally capture nay evidence of the httpdocs/js/lin/cccard.js file being edified but given the number of times others have done so on line we felit prudent to include it intros analysis.<br/>
<br/>
<br/></li>
</ul>
</li>
</ul>
</li>
<li><b>Magento Admin Panel Error Generated when you attempt to access it to login You are unable to login and get redirected over and over again in a loop to the login form .  The following message is logged to the var/app/log folder:</b> <span style="font-family: 'Courier New';">Error logging in the admin panel Fatal error: Class 'Magpleasure_Filesystem_Helper_Data' not found …/app/Mage.php on line 546</span>
<ul>
<li><a href="http://magento.stackexchange.com/questions/64461/error-logging-in-the-admin-panel-fatal-error-class-magpleasure-filesystem-help">http://magento.stackexchange.com/questions/64461/error-logging-in-the-admin-panel-fatal-error-class-magpleasure-filesystem-help<br/>
<br/>
<br/></a></li>
</ul>
</li>
<li><b>Discover of APC caching and / or Magento compilation are enabled on your system even though you did not enable it in PHP settings, configure APC in your original httpdocs/app/etc/local.xml or setup compilation in the Magento Admin console. </b>
<ul>
<li>During our investigations we determined that some Merchants suspected that they had been attacked in some way and potentially compromised.  Several searched the Internet and edified some of the articles and discussions threads reference in this document.  Finding suspsicious files on their system  they deleted them from the file system only to note that this had not apparent affect.  Pid closer examination and diff’s run against the source repos and the files system in place, we identified additional changes made to confirmations as well as parameters stored in the database that control directive like compilation. </li>
<li><b>These changes to the environment allowed the attackers to avoid detection for an extended period while the Merchant and ourselves were chains down multi exam for additional time while securing access to additional privileged data form the Magento stores.  After diff’ing the deployed files against a few known good copy of the source code form the client’s git repository, we were able to identify not only the changed files but also the files hat had been added  and removed from the deployment directory of their Magento App.<br/>
<br/>
<br/></b></li>
</ul>
</li>
<li><b>Discover newly created Admin Users added to Magneto admin with Full Admin role assignment and associated permissions so that the attackers could open up a browser an manually loving (or via a programmed bot) and extract additional details of historical transactions via MAgento’s built-in data export functions.</b>
<ul>
<li>Typically named something like : <b>admin_hacj oe admi_etqg</b></li>
<li>Accomplished either via an exploited SQL Injection Vulnerability not yet resolved by installing the available SUPEE-5994 updates or </li>
<li>If any of the previous exploits provide access to the file system or shell environments due to privilege escalation or improperly secured file / directory permissions, by smily opening a shell and connecting to the database via the local mysql client.</li>
<li>We were able to confirm that both madhouse had been used but examining the local .mysql_history file on all sites and noting that on one of them, commands unrelated to code that was installed for the client had been executed during same window that we identified  other attacks being executed.</li>
<li>Capturing all of the log files as quickly as possible allowed us to test our hypothesis by correlating entries in the Apache, MySQL, Magento, Magento, OpenSSL SSlogs as well as the .mysql_history & .bash_history files in the users’s home directories in the user’s home directories when shell escalation was accomplished. </li>
</ul>
</li>
<li style="display:inline;list-style:none;">
<ul>
<li>Refeence : <a href="https://rackspeed.de/blog/bruteforce-attacken-und-hacks-von-magento-shops/">https://rackspeed.de/blog/bruteforce-attacken-und-hacks-von-magento-shops/</a></li>
<li>Reference: <a href="https://rackspeed.de/blog/bruteforce-attacken-und-hacks-von-magento-shops/">http://magentary.com/kb/magento-recovery-shoplift-vulnerability/</a>
<ul>
<li>Also, User ROLES are modified or created to grant additional privileges to  new users who are added during compromise</li>
<li>Will normally use the same domain name for email accounts but repeat the username portion for each added user so that on initial review the users appear to at least have the same email domain suffice to avoid detection.  closer examination reveals the attacker grabbed a sintle alreadyssociated with a user and used it for each of the additional accounts they created.<a href="https://rackspeed.de/blog/bruteforce-attacken-und-hacks-von-magento-shops/"><br/></a><br/>
<br/></li>
</ul>
</li>
</ul>
</li>
<li><u><b>File:</b></u> httpdocs/get.php <u><b>Related to Shoplift Attack:</b></u>
<ul>
<li><a href="http://magento.stackexchange.com/questions/67660/magento-hacked-even-after-applied-patch">http://magento.stackexchange.com/questions/67660/magento-hacked-even-after-applied-patch</a></li>
<li><u><b>NOTE:</b></u> We did not identify any of these files as having been compromised on the sites we evaluated so we do not have a signature analysis;  If you have a compromised version of this file, please share it so we can add the signature to our database and improve scan accuracy.<br/>
<br/>
<br/></li>
</ul>
</li>
<li><span style="outline: 0px; color: rgb(0, 0, 0); font-family: 'Open Sans'; font-size: 14px; font-variant: normal; font-weight: normal; letter-spacing: normal; orphans: auto; text-align: left; text-indent: 0px; text-transform: none; white-space: normal; widows: 1; word-spacing: 0px; -webkit-text-stroke-width: 0px; background-color: rgb(250, 250, 250);"><b>MySQL Driver is altered to allow further attacker access to underlying database</b></span>
<ul>
<li><a href="http://magentary.com/kb/magento-recovery-shoplift-vulnerability/">http://magentary.com/kb/magento-recovery-shoplift-vulnerability/</a></li>
<li><em style="outline: 0px; font-style: italic; color: rgb(0, 0, 0); font-family: 'Open Sans'; font-size: 14px; font-variant: normal; font-weight: normal; letter-spacing: normal; orphans: auto; text-align: left; text-indent: 0px; text-transform: none; white-space: normal; widows: 1; word-spacing: 0px; -webkit-text-stroke-width: 0px; background-color: rgb(250, 250, 250);">lib/Varien/Db/Adapter/Pdo/Mysql.php</em><span style="color: rgb(0, 0, 0); font-family: 'Open Sans'; font-size: 14px; font-style: normal; font-variant: normal; font-weight: normal; letter-spacing: normal; orphans: auto; text-align: left; text-indent: 0px; text-transform: none; white-space: normal; widows: 1; word-spacing: 0px; -webkit-text-stroke-width: 0px; float: none; background-color: rgb(250, 250, 250);"> file modified, so patch can not be applied seamlessly:</span></li>
<li><span style="color: rgb(0, 0, 0); font-family: 'Open Sans'; font-size: 14px; font-style: normal; font-variant: normal; font-weight: normal; letter-spacing: normal; orphans: auto; text-align: left; text-indent: 0px; text-transform: none; white-space: normal; widows: 1; word-spacing: 0px; -webkit-text-stroke-width: 0px; float: none; background-color: rgb(250, 250, 250);"><u><b>NOTE</b></u>: </span>We did not identify any of these files as having been compromised on the sites we evaluated so we do not have a signature analysis;  If you have a compromised version of this file, please share it so we can add the signature to our database and improve scan accuracy.<br/>
<br/>
<br/></li>
</ul>
</li>
<li><b>New file that is already compromised is installed at media / dhl / info.php</b>
<ul>
<li><a href="http://community.magento.com/t5/Programming-Questions/Hacked-CE-installation-media-dlh-info-php/td-p/5428">http://community.magento.com/t5/Programming-Questions/Hacked-CE-installation-media-dlh-info-php/td-p/5428</a></li>
<li>This file examines the post parameters passed when the file called through the URL and certain conditions are met it render the output of phpinfo() to the page provided the attacker with detailed information and setup of the current server to further provide an attack surface for executing additional compromises.<a href="http://community.magento.com/t5/Programming-Questions/Hacked-CE-installation-media-dlh-info-php/td-p/5428"><br/>
<br/>
<br/>
<br/></a></li>
</ul>
</li>
<li><b><span style="font-family: 'Open Sans';"><span style="background-color: rgb(250, 250, 250);">Cookie Added allowing attackers to control and hijack user sessions and intercept the data that is associated with those sessions during chekcout</span></span></b>
<ul>
<li><em style="outline: 0px; font-style: italic;">app/code/core/Mage/Cms/controllers/IndexController.php</em> file have a hijacking cookie key installed</li>
<li>http://magentary.com/kb/magento-recovery-shoplift-vulnerability/</li>
<li><u><b>NOTE:</b></u> We did not identify any of these files as having been compromised on the sites we evaluated so we do not have a signature analysis;  If you have a compromised version of this file, please share it so we can add the signature to our database and improve scan accuracy.</li>
<li><a href="http://magentary.com/kb/magento-recovery-shoplift-vulnerability/"><br/>
<br/></a></li>
</ul>
</li>
<li><b>FILES: (MULTIPLE): Attackers place content in var/package/ folder that will be used later to attack site via Magpleasure/Filesystem module</b> 
<ul>
<li style="display:inline;list-style:none;">
<ul>
<li><span style="font-family: 'Courier New';">File_System-1.0.0.xml</span> <i>-  contains references, hashes and payload information for files included in the Magpleasure_Fielsystem module that is compromised and is used to install the files for the compromised plugin in various locations on the system hard drive.<br/>
<br/></i></li>
</ul>
</li>
</ul>
</li>
<li><u><b>NOTE</b></u>: The following file has suspicious payload and date / timestamps similar to he above file though we have been unable to confirm it as an attack vector with current analsysis.
<ul>
<li><font face="Courier New">tmp/package.xml : contains payloads file and directory naes as well as hashes that are coped to specific folders in the file system and expected to perofrn further attacks ataingt the system</font>
<ul>
<li><contents><target name="mageskin"><dir name="install"><dir name="default"><file name="install.php" hash="3b7de2cf163f18aa521c050bb543084f" /><file name=".htaccess" hash="8052c42ab3b8aa06a3f5f788a4ddccc2" /></dir></dir></target><target name="mage"><dir name="var"><dir name="package"><file name="pack.php" hash="3b7de2cf163f18aa521c050bb543084f" /><file name=".htaccess" hash="8052c42ab3b8aa06a3f5f788a4ddccc2" /></dir></dir></target></contents></li>
</ul>
</li>
<li>NOTE : Dates of last file changes on all of these files are consistently the same day and the time stamps of the files are all VERY close to each other.  The attackers do not appear to be taking too strong of a precaution of hiding their tracks when it comes to adding files or modifying the date time stamp data to the compromised files.  Be when we diff the files reference in the package.xml they appear identical to the default Magento insulation folders.</li>
<li>The package.file contains the files, hash’s and file contents that get written to the hard drive via the MagePleasure file system extension.  This is the entry point for the compromise and if you don’t remove these then your site will be compromised again and again even if you patch it because the patch won’t prevent this functionality from being executed.</li>
<li>NEED TO Review file names listed in the package.xm and track down their location to verify if they still exist on the filesystem and determine:
<ul>
<li>Where did they come from </li>
<li>To Which directories were they copied</li>
</ul>
</li>
<li>NEED TO REVIEW ON WGI - think we blasted the var/ directory but not sure</li>
<li>STILL ON SIDEWINDERPUMPS<br/>
<br/>
<br/>
<br/></li>
</ul>
</li>
</ul>
</li>
<li><u><b><i><span style="font-size: 18px;">Confirmed Solutions</span><span style="font-size: 18px;"> to issues not </span><span style="font-size: 18px;">resolved</span><span style="font-size: 18px;"> by simply deleting </span><span style="font-size: 18px;">inspected</span><span style="font-size: 18px;"> </span><span style="font-size: 18px;">file</span></i></b></u><span style="font-size: 18px;"><b><u><i>s identified abov</i>e</u></b>:</span><br/>
 
<ul>
<li><a href="http://magentary.com/kb/magento-recovery-shoplift-vulnerability/">http://magentary.com/kb/magento-recovery-shoplift-vulnerability/<br/>
<br/></a></li>
<li><b><a href="https://magento.stackexchange.com/questions/65097/fatal-error-class-magpleasure-filesystem-helper-data-not-found/65119#65119?newreg=7687cba75bc14bc6a590ee7421f456fb">https://magento.stackexchange.com/questions/65097/fatal-error-class-magpleasure-filesystem-helper-data-not-found/65119#65119?newreg=7687cba75bc14bc6a590ee7421f456fb<br/>
<br/></a></b></li>
<li><b><a href="http://magento.stackexchange.com/questions/67660/magento-hacked-even-after-applied-patch">http://magento.stackexchange.com/questions/67660/magento-hacked-even-after-applied-patch<br/>
<br/></a></b></li>
<li><b><a href="http://magento.stackexchange.com/questions/64930/applied-patches-but-still-getting-a-vulnerable-message?rq=1">http://magento.stackexchange.com/questions/64930/applied-patches-but-still-getting-a-vulnerable-message?rq=1<br/>
<br/></a></b></li>
<li><a href="http://invisiblezero.net/magento-secure-your-webshop/"><b>http://invisiblezero.net/magento-secure-your-webshop/<br/></b><br/></a></li>
<li>Disable / Remove Downloader / Magento Connect since it’s a vector for exploit: <a href="http://magentary.com/kb/restrict-access-to-magento-downloader/">http://magentary.com/kb/restrict-access-to-magento-downloader/<br/>
<br/></a></li>
<li>Diff & Replace all files with either
<ol>
<li>Verified version from your Git Repository</li>
<li>Fresh versions from Magento repository</li>
<li><a href="https://blog.tigertech.net/posts/cleaning-compromised-web-sites/">https://blog.tigertech.net/posts/cleaning-compromised-web-sites/<br/>
<br/></a></li>
</ol>
</li>
<li>NOT CONFIRMED : <a href="http://magento.stackexchange.com/questions/56130/migrating-hacked-magento-site">http://magento.stackexchange.com/questions/56130/migrating-hacked-magento-site<br/>
<br/></a></li>
<li>How to Respond Generally: <a href="https://blog.sucuri.net/2015/06/your-website-hacked-but-no-signs-of-infection.html">https://blog.sucuri.net/2015/06/your-website-hacked-but-no-signs-of-infection.html<br/>
<br/></a></li>
<li>How to Respond to Payment Processors & Banks: <a href="http://security.stackexchange.com/questions/89101/my-magento-shop-is-vulnerable-to-hacking">http://security.stackexchange.com/questions/89101/my-magento-shop-is-vulnerable-to-hacking</a><br/>
<br/>
<a href="https://rackspeed.de/blog/bruteforce-attacken-und-hacks-von-magento-shops/"><br/></a></li>
</ul>
</li>
<li>Additional Signatures to review
<ul>
<li> users with oAuth tokens assigned if you don’t use oAuth or have other users with them setup</li>
<li><a href="http://artistlab.co.uk/magento-shoplift-bug-supee-5344">http://artistlab.co.uk/magento-shoplift-bug-supee-5344</a> - Good Resource</li>
<li><a href="http://www.reddit.com/r/Magento/comments/33g9u9/urgent_fatal_error_class_magpleasure_filesystem/">http://www.reddit.com/r/Magento/comments/33g9u9/urgent_fatal_error_class_magpleasure_filesystem/</a></li>
<li><a href="http://www.reddit.com/r/Magento/comments/3441sc/modified_files_on_an_unpatched_production_site/">http://www.reddit.com/r/Magento/comments/3441sc/modified_files_on_an_unpatched_production_site/</a></li>
<li><a href="http://www.reddit.com/r/Magento/comments/35tumi/what_could_cause_this_vulnerability/">http://www.reddit.com/r/Magento/comments/35tumi/what_could_cause_this_vulnerability/</a><a href="https://rackspeed.de/blog/bruteforce-attacken-und-hacks-von-magento-shops/"><br/>
<br/></a></li>
</ul>
</li>
</ul>

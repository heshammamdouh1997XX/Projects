# Introduction:

In this article, I will show you how to configure file integrity monitoring (FIM) with best practices and examples with advanced techniques to detect more details.

- We have cent-OS that has the wazuh server and windows server 2016 that will has wazauh agent.

## At the Agent:

Make backup copy for ossec.conf just in case.
<p align="center">
    <img src="./images/Wazuh FIM-1.webp" width="500" height="500" />
</p>

Open ossec.conf and make sure File integrity is enabled.

- **disabled no == enabled**
<p align="center">
    <img src="./images/Wazuh FIM-2.webp" width="500" height="500" />
</p>

Then go to frequency to make it update the logs every 15 seconds.
<p align="center">
    <img src="./images/Wazuh FIM-3.webp" width="500" height="500" />
</p>

and to update the timing we need to restart the wazuh agent service and to do this you need to open search windows bar type **services press enter.**
<p align="center">
    <img src="./images/Wazuh FIM-4.webp" width="500" height="500" />
</p>

then restart the service.

We will monitor this directory for any files **C:\Users\Public** so create it in Default files to be monitored.

recursion_level=”0" => this attribute to make the focus on which directory inside the directory; so we don’t need it. (Don’t forget to save the file)

`<directories > C:\Users\Public </directories>`

<p align="center">
    <img src="./images/Wazuh FIM-5.webp" width="500" height="500" />
</p>

Restart the Wazuh service again now any body creates file there in any Public Directory will be recorded and will be inside the FIM

Note: If it didn’t work , then make it

`<directories recursion_level="1">C:\Users\Public</directories>`

- we will create a file and remove file from there and see it in the FIM on the server.

## At the Server:

Press Endpoint Security >> File Integrity Monitoring >> Inventory >> select agent (your windows)
<p align="center">
    <img src="./images/Wazuh FIM6.webp" width="500" height="500" />
</p>

press events:
<p align="center">
    <img src="./images/Wazuh FIM-7.webp" width="500" height="500" />
</p>

and press inspect on the left of the event deleted to get more information.
<p align="center">
    <img src="./images/Wazuh FIM-8.webp" width="100" height="100" />
</p>

and you can use md5 for example to check from virus-total.
<p align="center">
    <img src="./images/Wazuh FIM-9.webp" width="500" height="500" />
</p>

---

Also you will work on advanced settings to get more info. it’s called** Who-data Monitoring**. but first let’s make it** real-time** directory log by using this line instead of the old one.

`<directories recursion_level="1" realtime="yes">C:\Users\Public</directories>`

Then restart the wazuh service. and create a file.
<p align="center">
    <img src="./images/Wazuh FIM-10.webp" width="500" height="500" />
</p>

now we need more info so let’s remove the last line and put this line to get the **who data monitoring. and who-data is by default real-time. and also report the changes.**

`<directories check_all="yes" whodata="yes" report_changes="yes">C:\Users\Public</directories>`

---
**Back to windows:**

and also we need to change some audit options in the **windows** in case you are not windows 10, 11 or windows server 2019 or above.

open windows search bar > edit group policy (using Admin permission)

if you couldn’t -> use powershell administration permission and type

`gpedit.msc`

<p align="center">
    <img src="./images/Wazuh FIM-11.webp" width="500" height="500" />
</p>
If your system doesn’t allow configuring subcategories through Advanced Audit Policy Configuration, configure the **Security Setting** field to **Success** for the following policy:

- **Computer Configuration > Windows Settings > Security Settings > Local Policies > Audit Policy > Audit object access**
<p align="center">
    <img src="./images/Wazuh FIM-12.webp" width="500" height="500" />
</p>

A system access control list (SACL) enables administrators to log attempts to access a secured object. You can check and modify SACLs of each monitored directory through **Properties**, selecting the **Security** tab, and clicking on **Advanced**:
<p align="center">
    <img src="./images/Wazuh FIM-13.webp" width="500" height="500" />
</p>

It’s necessary to have a **Success** entry in the Auditing tab:
<p align="center">
    <img src="./images/Wazuh FIM-13.webp" width="500" height="500" />
</p>

if no , press **add** > **select principle** then press **check names**
<p align="center">
    <img src="./images/Wazuh FIM-14.webp" width="500" height="500" />
</p>
press show advanced permissions and mark like the photo.
<p align="center">
    <img src="./images/Wazuh FIM-15.webp" width="500" height="500" />
</p>

tell you see success or if you have it from the beginning then you don’t need to change the permission .
<p align="center">
    <img src="./images/Wazuh FIM-16.webp" width="500" height="500" />
</p>

then restart the service again.

**Troubleshooting tip**: sometimes it will need to do the same steps twice especially when you are doing Audit Object Access.

I created a file and modified it .

---
## In Wazuh Server :

Now ,we got the changed text in the file in the first photo. and and auditing details in the second photo.
<p align="center">
    <img src="./images/Wazuh FIM-17.webp" width="500" height="500" />
</p>
the changing in the text
<p align="center">
    <img src="./images/Wazuh FIM-18.webp" width="500" height="500" />
</p>
Auditing Details

## Conclusion:

In this small project we configured File Integrity Monitoring using Wazuh and did an advanced settings to get more details in log.
## Reference:
Advanced settings - File integrity monitoring · Wazuh documentation
Check out this section to learn about different advanced settings that can provide greater control and flexibility over…

documentation.wazuh.com

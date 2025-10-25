# part 3 configuring the hive and shuffle

## Shuffle
got to this site and create an account

https://shuffler.io/register?message=Get+started+for+free&source=post_page-----ca04992c5d98---------------------------------------

go to workfows
<p align="center">     <img src="./images/part3-conf-hive-shuff-1.webp" width="500" height="500"  /> </p>
create new workflow
<p align="center">     <img src="./images/part3-conf-hive-shuff-2.webp" width="500" height="500"  /> </p>
choose trigger and drag it into the board like in the screenshot 
<p align="center">     <img src="./images/part3-conf-hive-shuff-3.webp" width="500" height="500"  /> </p>
click on webhook and then name it and copy the url 
<p align="center">     <img src="./images/part3-conf-hive-shuff-4.webp" width="500" height="500"  /> </p>
then press change me icon and press on the plus icon like the screenshot and choose execution argument

<p align="center">     <img src="./images/part3-conf-hive-shuff-5.webp" width="500" height="500"  /> </p>
then save it
<p align="center">     <img src="./images/part3-conf-hive-shuff-6.webp" width="500" height="500"  /> </p>

let's go to the server to tell wazuh to connect to shuffle.
lets go to 

`nano /var/ossec/etc/ossec.conf`
then paste it after the global conf.
<p align="center">     <img src="./images/part3-conf-hive-shuff-7.webp" width="500" height="500"  /> </p>

```
  <integration>
    <name>shuffle</name>
    <hook_url>https://shuffler.io/api/v1/hooks/webhook(your id is here) </hook_url>    
    <level>3</level>
    <alert_format>json</alert_format>
  </integration>
```
**tip : after your http-url in the intigration put space and make sure hook_url is white. and also focus with the spaces if you don't know you can see the tags like global or alerts and do the same spaces.**

<level> => will make you send all the level 5 to shuffle but we need specefic id to trigger just the mimikatz so we will change it.
we will use rule_id
<p align="center">     <img src="./images/part3-conf-hive-shuff-8.webp" width="500" height="500"  /> </p>
and restart the server
test the workflow
<p align="center">     <img src="./images/part3-conf-hive-shuff-9.webp" width="500" height="500"  /> </p>
excute it using play button go and open mimikatz
it works… 
<p align="center">     <img src="./images/part3-conf-hive-shuff-10.webp" width="500" height="500"  /> </p>
then press plus button for details
<p align="center">     <img src="./images/part3-conf-hive-shuff-11.webp" width="500" height="500"  /> </p>

### the next work flow automation
1- mimkatz alert sent to shuffle.
2- shffle receives mimikatz alert.
3- check reputation score with virus total.
4- send details to the hive.
5- send email to soc analyst to begin investigation.

extract the hash first from change me icon 
make the find actions (regex capture group)
<p align="center">     <img src="./images/part3-conf-hive-shuff-12.webp" width="500" height="500"  /> </p>
<p align="center">     <img src="./images/part3-conf-hive-shuff-13.webp" width="500" height="500"  /> </p>
<p align="center">     <img src="./images/part3-conf-hive-shuff-14.webp" width="500" height="500"  /> </p>
you can use AI like chat-gpt to generate the regex.
then rerun 
<p align="center">     <img src="./images/part3-conf-hive-shuff-15.webp" width="500" height="500"  /> </p>
<p align="center">     <img src="./images/part3-conf-hive-shuff-16.webp" width="500" height="500"  /> </p>
now we got the sha256
create an account at virus total and take the api
<p align="center">     <img src="./images/part3-conf-hive-shuff-17.webp" width="500" height="500"  /> </p>
<p align="center">     <img src="./images/part3-conf-hive-shuff-18.webp" width="500" height="500"  /> </p>

do auth with virus total v3
put your virustotal api key and the url of virustotal
<p align="center">     <img src="./images/part3-conf-hive-shuff-19.webp" width="500" height="500"  /> </p>
for input make sure you press on list to choose the right group
<p align="center">     <img src="./images/part3-conf-hive-shuff-20.webp" width="500" height="500"  /> </p>
<p align="center">     <img src="./images/part3-conf-hive-shuff-21.webp" width="500" height="500"  /> </p>
save and check the workflow but you will find the error
<p align="center">     <img src="./images/part3-conf-hive-shuff-22.webp" width="500" height="500"  /> </p>
scroll down in the report and you cn see in attributes > last_analysis_status
you will see malicious : 62
<p align="center">     <img src="./images/part3-conf-hive-shuff-23.webp" width="500" height="500"  /> </p>
then search for thehive in the apps search bar then grap it to the workflow
<p align="center">     <img src="./images/part3-conf-hive-shuff-24.webp" width="500" height="500"  /> </p>

### in the hive machine:

**http://ip:9000**

#### default credential:

```
username: admin@thehive.local
pass: secret
```

**But we need to be online so we can communicate with shuffle cloud So we will use ngrok**

[https://ngrok.com/]

After you sign up you should follow the steps to create your own HTTPS
after I 've done that 

`ngrok http http://myipaddress:9000`
<p align="center">     <img src="./images/part3-conf-hive-shuff-25.webp" width="500" height="500"  /> </p>
<p align="center">     <img src="./images/part3-conf-hive-shuff-26.webp" width="500" height="500"  /> </p>
<p align="center">     <img src="./images/part3-conf-hive-shuff-27.webp" width="500" height="500"  /> </p>
<p align="center">     <img src="./images/part3-conf-hive-shuff-28.webp" width="500" height="500"  /> </p>
we are online and in now . but before that make sure that you changed the password.
press the plus button to create new org.
<p align="center">     <img src="./images/part3-conf-hive-shuff-29.webp" width="500" height="500"  /> </p>
<p align="center">     <img src="./images/part3-conf-hive-shuff-30.webp" width="500" height="500"  /> </p>
go to the security org then create 2 users using the plus button.
<p align="center">     <img src="./images/part3-conf-hive-shuff-31.webp" width="500" height="500"  /> </p>
put the data
<p align="center">     <img src="./images/part3-conf-hive-shuff-32.webp" width="500" height="500"  /> </p>
and create the another user for shuffle
<p align="center">     <img src="./images/part3-conf-hive-shuff-33.webp" width="500" height="500"  /> </p>
now we should change the password and create api key for the shuffle service.
press preview eye
<p align="center">     <img src="./images/part3-conf-hive-shuff-34.webp" width="500" height="500"  /> </p>
then set new password
Do the same thing in shuffle service but this time create an api key
<p align="center">     <img src="./images/part3-conf-hive-shuff-35.webp" width="500" height="500"  /> </p>
log out and log in using your account in my case Hisham@test.com
<p align="center">     <img src="./images/part3-conf-hive-shuff-36.webp" width="500" height="500"  /> </p>
now go back to the shuffle 
- make the action is create alert
<p align="center">     <img src="./images/part3-conf-hive-shuff-37.webp" width="500" height="500"  /> </p>
- authenticate (put the api key from shuffle in the hive) then the url
<p align="center">     <img src="./images/part3-conf-hive-shuff-38.webp" width="500" height="500"  /> </p>
connect the hive with virustotal
<p align="center">     <img src="./images/part3-conf-hive-shuff-39.webp" width="500" height="500"  /> </p>
put the data , you want in the hive like this:

```
Description: $exec.text.win.eventdata.description
flag: false
pap:2
source: $exec.text.win.system.computer
sourceref: $exec.rule_id
tlp (Traffic light protocol-> confidentialitiy of info): 2
status: new
type: internal
```

save the workflow



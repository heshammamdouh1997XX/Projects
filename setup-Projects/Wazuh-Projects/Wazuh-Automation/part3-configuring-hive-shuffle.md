# part 3 configuring the hive and shuffle

## Shuffle
got to this site and create an account

https://shuffler.io/register?message=Get+started+for+free&source=post_page-----ca04992c5d98---------------------------------------

go to workfows
***
create new workflow
***
choose trigger and drag it into the board like in the screenshot 
***
click on webhook and then name it and copy the url 
***
then press change me icon and press on the plus icon like the screenshot and choose execution argument

***
then save it
***

let's go to the server to tell wazuh to connect to shuffle.
lets go to 

`nano /var/ossec/etc/ossec.conf`
then paste it after the global conf.
***
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
***
and restart the server
test the workflow
***
excute it using play button go and open mimikatz
it works… 
***
then press plus button for details
***
### the next work flow automation
1- mimkatz alert sent to shuffle.
2- shffle receives mimikatz alert.
3- check reputation score with virus total.
4- send details to the hive.
5- send email to soc analyst to begin investigation.

extract the hash first from change me icon 
make the find actions (regex capture group)
***
***
***
you can use AI like chat-gpt to generate the regex.
then rerun 
***
***
now we got the sha256
create an account at virus total and take the api
***
***

do auth with virus total v3
put your virustotal api key and the url of virustotal
***
for input make sure you press on list to choose the right group
***
***
save and check the workflow but you will find the error
***
scroll down in the report and you cn see in attributes > last_analysis_status
you will see malicious : 62
***
then search for thehive in the apps search bar then grap it to the workflow
***
in the hive machine:
**http://ip:9000**
#### default credential:
```
username: admin@thehive.local
pass: secret
```
**But we need to be online so we can communicate with shuffle cloud So we will use ngrok
**
https://ngrok.com/

After you sign up you should follow the steps to create your own HTTPS
after I 've done that 

`ngrok http http://myipaddress:9000`
***
***
***
***
we are online and in now . but before that make sure that you changed the password.
press the plus button to create new org.
***
***
go to the security org then create 2 users using the plus button.
***
put the data
***
and create the another user for shuffle
***
now we should change the password and create api key for the shuffle service.
press preview eye
***
then set new password
Do the same thing in shuffle service but this time create an api key
***
log out and log in using your account in my case Hisham@test.com
***
now go back to the shuffle 
- make the action is create alert
***
- authenticate (put the api key from shuffle in the hive) then the url
***
connect the hive with virustotal
***
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



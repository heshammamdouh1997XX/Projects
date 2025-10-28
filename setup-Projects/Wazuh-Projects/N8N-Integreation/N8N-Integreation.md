# N8N-Integreation with Wazuh

You need to Download those 2 files in the directory `/var/ossec/integrations` this link : https://github.com/maikroservice/wazuh-integrations/blob/main/n8n/custom-n8n.py

- Download custom-n8n -> to connect wazuh with the python code (custom-n8n.py).

- Download custom-n8n.py -> to send the information from wazuh to n8n server.
```
sudo chown root:wazuh custom-n8n.py
sudo chmod 750 custom-n8n.py

sudo chown root:wazuh custom-n8n
sudo chmod 750 custom-n8n
```
  

- Restart the wazuh Manager.

## at n8n:
Use the webHook and make it testing for the POST request.
take the URL and put it in the Integration Area <hook_url>

 ## in the conf server at Manager configuration:

 ```xml
    <integration>
    <name>custom-n8n</name>
    <hook_url>https://<username>.app.n8n.cloud/webhook-test/<id></hook_url>  
    <rule_id>100002</rule_id>
    <level> 3 </level>
    <alert_format>json</alert_format>
  </integration>

```



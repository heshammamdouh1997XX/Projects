# Suricata Intigeration

1- Add the Linux Agent to Wazuh.

2- At the Agent, `Nano /var/ossec/etc/ossec.conf`

3- add this code before the end
```
  <localfile>
    <log_format>syslog</log_format>
    <location>/var/log/suricata/eve.json</location>
  </localfile>
```

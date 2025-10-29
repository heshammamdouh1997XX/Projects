# Wazuh Trust the cert

- To test the domain
`curl -v https://n8n.mycompany.local/webhook/test`

## Option A — Trust the cert system-wide (recommended)

1. Copy your CA root certificate or n8n.crt to:
`/usr/local/share/ca-certificates/n8n.crt`
2. Update the system CA store:
 `sudo update-ca-certificates`
3. Restart Wazuh manager to refresh its environment:
   `sudo systemctl restart wazuh-manager`

**Then verify it’s trusted:**

`openssl verify -CAfile /etc/ssl/certs/ca-certificates.crt mycompany-rootCA.crt`

### Troubleshoot
`grep -R shuffle /var/ossec/logs/`

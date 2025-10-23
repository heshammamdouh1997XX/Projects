# Suricata Intigration (On UBUNTU) with Wazuh


## Install Suricata

Setup to install the latest stable Suricata:
```
sudo apt update
sudo apt-get install software-properties-common
sudo add-apt-repository ppa:oisf/suricata-stable
sudo apt-get update
```

Then, you can install the latest stable with:

`sudo apt-get install suricata`

## Control Suricata
```
sudo systemctl status suricata
sudo systemctl enable suricata
sudo systemctl start suricata
sudo systemctl stop suricata
```

## Configure Suricata
`sudo nano /etc/suricata/suricata.yaml`



## Signatures
`sudo suricata-update`



## Upgrading Suricata
sudo apt-get update
sudo apt-get upgrade suricata

## Getting Debug or Pre-release Versions
`sudo apt-get install suricata-dbg`






## Reference
https://docs.suricata.io/en/suricata-8.0.1/install/ubuntu.html

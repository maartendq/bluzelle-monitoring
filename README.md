# bluzelle-monitoring
## To change
prometheus/sd/mainnet or prometheus/sd/testnet: Define your own hostnames/IPs in the yml files.

alertmanager/alertmanager.yml:
smtp_smarthost: Mailhost for sending out mails
Receivers - to: Mail address to send alerts to
 
## How to
Start your stack: ./provision.sh
Stop your stack: docker-compose -f docker-compose.yml down --volumes (Run without --volumes if  you want to keep data)

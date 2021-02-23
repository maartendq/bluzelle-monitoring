# bluzelle-monitoring
## To change

What is bluzelle-monitoring?
A stack of tools to monitor <a href="https://www.bluzelle.com/">Bluzelle</a> validators/sentries/nodes.

Dependencies and prerequisites.

On the host running blzd
in .blzd/config/config.toml set ```promentheus = true``` - restart blzd to reload the config file.
Install prometheus-node-exporter
Allow access on ports 9100 and 26660 from the monitoring host.

On the monitoring host
install docker-compose git
clone this repo
open ports 3000 and 8080
Edit the files prometheus/sd/mainnet and/or prometheus/sd/testnet. Define your hostnames/IPs in the yml files.
Edit alertmanager/alertmanager.yml, define your smtp credentials and set the desired email_configs:
Edit docker-compose.yml, Remove the # and set admin passwords for alerta, line 44 and 45.


## How to deploy
Run ./provision.sh (will autostart)
Start your stack docker-compose start [SERVICE]
Stop your stack docker-compose stop [SERVICE]
Stop and purge your stack: docker-compose -f docker-compose.yml down --volumes (Run without --volumes if  you want to keep data

## URLS
http://host.ip.host.ip:3000 Grafana (password admin/admin)
http://host.ip.host.ip:8080 alerta

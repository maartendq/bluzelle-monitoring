# bluzelle-monitoring


### What is bluzelle-monitoring?
A stack of tools to monitor <a href="https://www.bluzelle.com/">Bluzelle</a> validators/sentries/nodes.

#### Dependencies and prerequisites.

**On the hosts running blzd:**

* in ```.blzd/config/config.toml``` set ```promentheus = true``` - restart blzd to reload the config file.
* Install prometheus-node-exporter
* Allow network access on ports 9100 and 26660 from the monitoring host.

**On the monitoring host:** 

* Install docker-compose
* Install git
* Clone this repo
* Define your hostnames/IPs in prometheus/sd/mainnet and/or prometheus/sd/testnet yaml files
* Edit alertmanager/alertmanager.yml, define your smtp credentials and set the desired email_configs:
* Edit docker-compose.yml, Remove # and set admin passwords for alert, line 44 and 45.

#### Warning for hosts using UFW as a firewall
Docker will create wide open iptables rules for inbound traffic to the containers which will bypass any current UFW blocks/rejects. To block inbound traffic to the containers append `/etc/ufw/after.rules` with 

```
# BEGIN UFW AND DOCKER
*filter
:ufw-user-forward - [0:0]
:DOCKER-USER - [0:0]
-A DOCKER-USER -j RETURN -s 10.0.0.0/8
-A DOCKER-USER -j RETURN -s 172.16.0.0/12
-A DOCKER-USER -j RETURN -s 192.168.0.0/16

-A DOCKER-USER -j ufw-user-forward

-A DOCKER-USER -j DROP -p tcp -m tcp --tcp-flags FIN,SYN,RST,ACK SYN -d 192.168.0.0/16
-A DOCKER-USER -j DROP -p tcp -m tcp --tcp-flags FIN,SYN,RST,ACK SYN -d 10.0.0.0/8
-A DOCKER-USER -j DROP -p tcp -m tcp --tcp-flags FIN,SYN,RST,ACK SYN -d 172.16.0.0/12
-A DOCKER-USER -j DROP -p udp -m udp --dport 0:32767 -d 192.168.0.0/16
-A DOCKER-USER -j DROP -p udp -m udp --dport 0:32767 -d 10.0.0.0/8
-A DOCKER-USER -j DROP -p udp -m udp --dport 0:32767 -d 172.16.0.0/12

-A DOCKER-USER -j RETURN
COMMIT

# END UFW AND DOCKER
```
Use `ufw route allow` to allow selected traffic.

#### How to deploy

* ./provision.sh will autostart the entire stack
* Stop `docker-compose -f docker-compose.yml down`
* Stop and purge all data `docker-compose -f docker-compose.yml down --volumes`

#### URLS

http://host.ip.host.ip:3000 Grafana  
http://host.ip.host.ip:8080 Alerta   
http://host.ip.host.ip:9090 Prometheus  
http://host.ip.host.ip:9093 Alertmanager

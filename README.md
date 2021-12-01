# **Nagios-Promethues-AlertManager**

Alert service for nginx server with [Prometheus](https://prometheus.io), [Alert Manager](https://eur03.safelinks.protection.outlook.com/?url=https%3A%2F%2Fprometheus.io%2Fdocs%2Falerting%2Flatest%2Falertmanager%2F&data=04%7C01%7C%7C512d68de3bf441bb380d08d9aabcb71c%7C92bd06fd7d0b494a8f770e63d2bf3a3e%7C0%7C0%7C637728549774821540%7CUnknown%7CTWFpbGZsb3d8eyJWIjoiMC4wLjAwMDAiLCJQIjoiV2luMzIiLCJBTiI6Ik1haWwiLCJXVCI6Mn0%3D%7C3000&sdata=n%2BISfmEoqdXAp4viS%2Fz%2BXu7nLhu32dwiTlyTt72UQb4%3D&reserved=0) using [Docker](https://eur03.safelinks.protection.outlook.com/?url=https%3A%2F%2Fwww.docker.com%2F&data=04%7C01%7C%7C512d68de3bf441bb380d08d9aabcb71c%7C92bd06fd7d0b494a8f770e63d2bf3a3e%7C0%7C0%7C637728549774821540%7CUnknown%7CTWFpbGZsb3d8eyJWIjoiMC4wLjAwMDAiLCJQIjoiV2luMzIiLCJBTiI6Ik1haWwiLCJXVCI6Mn0%3D%7C3000&sdata=5Z4IqM4zh2dyUBuQB3sg1YZvliWPE7cdAnfJoLihDN4%3D&reserved=0) and [Docker Compose](https://eur03.safelinks.protection.outlook.com/?url=https%3A%2F%2Fdocs.docker.com%2Fcompose%2F&data=04%7C01%7C%7C512d68de3bf441bb380d08d9aabcb71c%7C92bd06fd7d0b494a8f770e63d2bf3a3e%7C0%7C0%7C637728549774831502%7CUnknown%7CTWFpbGZsb3d8eyJWIjoiMC4wLjAwMDAiLCJQIjoiV2luMzIiLCJBTiI6Ik1haWwiLCJXVCI6Mn0%3D%7C3000&sdata=xU3Y28dx284qvJYaZC9w8qfcgEwD0NSSAVmXrCsZbFU%3D&reserved=0).
___

## **Install**
Clone this repository on your Docker host, cd into Nagios-Promethues-SRE directory and run compose up:
```
git clone https://github.com/ShanMarshBubashan/Nagios-Promethues-SRE.git
cd Nagios-Promethues-SRE

docker-compose up -d
```
#### **Prerequisites:**
* Docker Engine >= 1.13
* Docker Compose >= 1.11

#### **Pre-configuration:** 
Configure the `.env` file to setup the host_ip, port, docker image version and container name of the setup or use the default used.
___

## **Usage**

### _nginx Homepage_
nginx homepage : [localhost:{{portnumber}}](http://localhost:80) (Port Number 80 used as default)

![nginx Homepage](https://github.com/ShanMarshBubashan/Nagios-Promethues-SRE/blob/master/Screenshots/nagios.png)

### _Prometheus Homepage_
Promethues homepage : [localhost:{{portnumber}}](http://localhost:9090) (Port Number 9090 used as default)
![Prometheus Homepage](https://github.com/ShanMarshBubashan/Nagios-Promethues-SRE/blob/master/Screenshots/Prometheus.png)

### _Alert Manager Homepage_
Alert Manager homepage : [localhost:{{portnumber}}](http://localhost:9093) (Port Number 9093 used as default)
![Alert Manager Homepage](https://github.com/ShanMarshBubashan/Nagios-Promethues-SRE/blob/master/Screenshots/Alert%20manager.png)
___

## **Define Alerts**

One Alert been setup within the [alert.rules](https://github.com/ShanMarshBubashan/Nagios-Promethues-SRE/blob/master/.env) configuration file, to monitor nagios server status:

#### **Alert Setup**
Trigger an alert if any of the monitoring targets (node-exporter and cAdvisor) are down for more than 30 seconds:
```
- alert: InstanceDown  
        expr: nginx_up == 0
        for: 30s
        labels:
          severity: "critical"
        annotations:
          summary: "Endpoint {{ $labels.instance }} down"
```

___
## **Alert Setup**
The AlertManager service is responsible for handling alerts sent by Prometheus server. A complete list of integrations can be found in [here](https://github.com/stefanprodan/dockprom).

The notification receivers can be configured in [alertmanager/config.yml file](https://github.com/ShanMarshBubashan/Nagios-Promethues-SRE/blob/master/alertmanager/config.yml).

```
route:
  receiver: 'slack'

receivers:
  - name: 'slack'
```
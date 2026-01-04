# 🛡️ CyberSecOps Lab: SIEM-Driven Network Forensics & Threat Hunting

## 📌 Overview
This project is a hands-on cybersecurity lab focused on **Networking and Security Operations**, combining **network traffic analysis**, **protocol troubleshooting**, **SIEM-based log correlation**, **incident forensics**, and **threat hunting**.

A real-world **SSH brute-force attack scenario** was simulated and analyzed using **Wireshark** and the **ELK Stack (Elasticsearch, Logstash, Kibana, Filebeat)** to understand how Security Operations Centers (SOCs) detect, investigate, and respond to security incidents.

---

## 🎯 Project Scope
- Subnet design and network setup
- Network traffic capture and analysis
- Protocol troubleshooting (SSH failures)
- SIEM setup and log correlation using ELK Stack
- Incident forensics and threat hunting

---
## 🧰  Tools & Technologies
- Kali Linux  
- Ubuntu Server  
- Wireshark  
- ELK Stack (Elasticsearch, Logstash, Kibana, Filebeat)  
- Linux networking tools (`ping`, `ssh`)  
- Networking protocols (TCP/IP, SSH)



##🌐 Network Traffic Capture
- Traffic captured for **over 10 minutes** using **Wireshark**
- Both **normal activity** and **attack simulation** traffic recorded
- Focus on detecting anomalies during attack scenarios

---

## 🔧 Network Protocol Troubleshooting

###⚠️ Issue Simulated
- SSH connection failures
- Repeated authentication errors



---

## 🚨  Incident Simulation & Network Forensics

###📝 Incident Description
An **SSH brute-force attack** was simulated from a **Kali Linux VM** targeting an **Ubuntu Server**.

###⚔️ Attack Method
- Repeated SSH login attempts
- Multiple invalid usernames
- No successful authentication

###🔍  Network Forensics (Wireshark)
- TCP traffic on **port 22** identified
- Repeated SYN packets from attacker IP
- Failed SSH handshake attempts observed

---

##📊  SIEM Setup Using ELK Stack

### Components Installed
- Elasticsearch **7.17.29**
- Logstash
- Kibana
- Filebeat

---

##⚙️ Installation and Configuration (Ubuntu Server VM)

###✅ Prerequisites
- Ubuntu Server
- Java JDK 11 or higher installed

---

### Elasticsearch Installation

```bash
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
sudo apt install -y apt-transport-https
echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee /etc/apt/sources.list.d/elastic-7.x.list
sudo apt update
sudo apt install -y elasticsearch
```
Configure /etc/elasticsearch/elasticsearch.yml:
```bash
network.host: <ubuntu_ip>
```
Start and enable:
```bash
sudo systemctl start elasticsearch
sudo systemctl enable elasticsearch
```
Verify:
```bash
curl http://<ubuntu_ip>:9200

```
Kibana Installation
```bash
sudo apt install -y kibana
```
Configure /etc/kibana/kibana.yml:
```bash
server.host: "<ubuntu_ip>"
elasticsearch.hosts: ["http://<ubuntu_ip>:9200"]
```
Start and enable:
```bash
sudo systemctl start kibana
sudo systemctl enable kibana
```
Access:
```bash
http://<ubuntu_ip>:5601
```
Logstash Installation
```bash
sudo apt install -y logstash
```
Create configuration file:
```bash
sudo nano /etc/logstash/conf.d/01-syslog.conf
```
```text
input {
  syslog {
    port => 5514
  }
}

output {
  elasticsearch {
    hosts => ["<ubuntu_ip>:9200"]
  }
}
```
Start and enable:
```bash
sudo systemctl start logstash
sudo systemctl enable logstash
```
Filebeat Installation & Configuration:
```bash
sudo apt install -y filebeat
```
Edit /etc/filebeat/filebeat.yml:
```text
filebeat.inputs:
- type: log
  enabled: true
  paths:
    - /var/log/auth.log

output.logstash:
  hosts: ["<ubuntu_ip>:5044"]
```
Start and enable:
```bash
sudo systemctl start filebeat
sudo systemctl enable filebeat
```
📂Indexes Created in Kibana:
- filebeat-*
- logstash-syslog-*

🕵️Sample Kibana Query (Threat Detection) For example:
```bash
event.action:"ssh_login" AND event.outcome:"failure" AND source.ip:"192.168.1.0/24"

```
📊Final Dashboard's Visualizations
- SSH login failures
- Top source IP addresses
- Failed SSH login attempts over time

📌Findings:
- Multiple failed SSH login attempts detected
- Attacker identified through network traffic and SIEM logs
- Evidence confirmed in /var/log/auth.log
- Incident successfully analyzed and contained

🎓Learning Outcomes:
- Practical SOC-style incident investigation
- SIEM log correlation and threat detection
- Network forensics using packet captures
- Threat hunting using logs and traffic analysis
- Understanding real-world cybersecurity operations

⚠️Disclaimer

This project was conducted in a controlled lab environment for educational purposes only.
No unauthorized systems were targeted.




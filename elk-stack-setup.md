# 🧠 Complete ELK Stack Setup on Ubuntu (Elastic, Logstash, Kibana)

### 👩‍💻 Author
**Rona Playda**  
[GitHub](https://github.com/ronaplayda4) • [LinkedIn](https://linkedin.com/in/rona-playda-705b5927b)

---
> ⚠️ **Security Note:**  
> All IP addresses, credentials, and tokens in this guide are placeholders for educational purposes only.  
> Do not reuse these values in a production environment.


## 📘 Project Overview
This guide documents my hands-on setup and configuration of the **Elastic Stack (ELK)** — **Elasticsearch**, **Logstash**, and **Kibana** — for log management, visualization, and security monitoring using **Fleet and Elastic Agent**.

---

## 🖥️ System Setup
- **OS:** Ubuntu Server 22.04 (VMware)
- **IP Address:** 198.168.xxx.xxx (Ubuntu)
- **Elasticsearch Version:** 8.17.4
- **Kibana Version:** 8.17.4
- **Logstash Version:** 8.17.4
- **Elastic Agent:** Installed later for Fleet and Windows endpoint log collection.

---

## ⚙️ Step 1: System Preparation

Update and install dependencies:
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install apt-transport-https curl wget gnupg -y

⚙️ Step 2: Install Elasticsearch
2.1 Import Elasticsearch GPG Key and Repository
curl -fsSL https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo gpg --dearmor -o /usr/share/keyrings/elasticsearch-keyring.gpg
echo "deb [signed-by=/usr/share/keyrings/elasticsearch-keyring.gpg] https://artifacts.elastic.co/packages/8.x/apt stable main" | sudo tee /etc/apt/sources.list.d/elastic-8.x.list

2.2 Install and Start Elasticsearch
sudo apt update && sudo apt install elasticsearch -y
sudo systemctl enable elasticsearch
sudo systemctl start elasticsearch
sudo systemctl status elasticsearch

2.3 Test Connection
curl -k -u elastic:'your_password' https://198.168.xxx.xxx:9200

Expected output:
{
  "name": "ubuntu-elastic-node",
  "cluster_name": "elk-cluster",
  "version": {
    "number": "8.17.4"
  },
  "tagline": "You Know, for Search"
}

⚙️ Step 3: (Optional) Install Logstash

Logstash is optional — Elastic Agent can also handle data collection.
Add this section to make it a full ELK setup.
sudo apt install logstash -y
sudo systemctl enable logstash
sudo systemctl start logstash
sudo systemctl status logstash

⚙️ Step 4: Install Kibana
4.1 Install and Enable Kibana
sudo apt install kibana -y
sudo systemctl enable kibana
sudo systemctl start kibana
sudo systemctl status kibana

4.2 Access Kibana

From your browser:
http://198.168.xxx.xxx:5601

Login with:
Username: elastic
Password: your_password

⚙️ Step 5: Configure Kibana Encryption Key

To avoid Fleet security warnings:
openssl rand -base64 48 | sudo tee -a /etc/kibana/kibana.yml
echo 'xpack.encryptedSavedObjects.encryptionKey: "<PASTE_RANDOM_KEY>"' | sudo tee -a /etc/kibana/kibana.yml
sudo systemctl restart kibana

🧠 Step 6: Verify Elasticsearch Indices
curl --cacert /etc/elasticsearch/http_ca.crt -u elastic:'your_password' \
"https://198.168.xxx.xxx:9200/_cat/indices?v"
(If only .internal.* indices appear → proceed to Fleet setup)

🚀 Step 7: Set Up Fleet and Elastic Agent
7.1 In Kibana
Go to Management → Fleet
Click Set up Fleet

7.2 Create an Agent Policy
Click Create agent policy
Name: Windows-Endpoints
Enable “Collect system logs and metrics”

7.3 Add Integrations
Add:
System
Windows
Elastic Defend

🧱 Step 8: Enroll Fleet Server (Ubuntu)

In Kibana → Fleet → Agents → Add agent → Enroll in Fleet (Quick Start)
Run the commands it provides (replace <TOKEN> with your token):

sudo ufw allow 8220/tcp
curl -L -O https://artifacts.elastic.co/downloads/beats/elastic-agent/elastic-agent-8.17.4-amd64.deb
sudo dpkg -i elastic-agent-8.17.4-amd64.deb

sudo elastic-agent install \
  --url=https://198.168.xxx.xxx:8220 \
  --enrollment-token=<YOUR_FLEET_ENROLLMENT_TOKEN> \
  --certificate-authorities=/etc/elasticsearch/http_ca.crt

Check status:
sudo elastic-agent status
(Once successful, go to Kibana → Fleet → Agents → you should see 1 healthy agent.)

🪟 Step 9: Enroll Windows Endpoint

On your Windows machine (PowerShell as Administrator):

Invoke-WebRequest -Uri https://198.168.xxx.xxx:8220/e/default/downloads/elastic-agent-8.17.4-windows-x86_64.msi -OutFile elastic-agent.msi
Start-Process msiexec.exe -Wait -ArgumentList "/i elastic-agent.msi /qn"
& "C:\Program Files\Elastic\Agent\elastic-agent.exe" install `
  --url=https://198.168.xxx.xxx:8220 `
  --enrollment-token=<WINDOWS_ENROLLMENT_TOKEN> `
  --certificate-authorities="C:\Program Files\Elastic\Agent\http_ca.crt"

Wait 1-2 minutes, then check Kibana - Fleet - Agents for Windows host.

📊 Step 10: View Data in Kibana

Go to Analytics → Discover
Create a new Data View
Pattern: logs-*
Timestamp field: @timestamp
Click Save as default
Set time range to Last 24 hours

✅ You should now see logs from Elastic Agent and your Windows machine.

🔎 Step 11: Verify Data in Elasticsearch
curl --cacert /etc/elasticsearch/http_ca.crt -u elastic:'your_password' \
"https://198.168.xxx.xxx:9200/_cat/indices/logs-*,metrics-*?v"

Expected indices:
logs-elastic_agent.*, logs-windows.*, metrics-system.*

🎓 Educational Purpose

This setup demonstrates how to deploy a complete ELK Stack environment in a virtualized home lab.
Users can use it to learn log analysis, SIEM concepts, and endpoint monitoring — core skills for SOC and cybersecurity careers.

🏁 Conclusion

✅ Elasticsearch, Kibana, and Logstash installed
✅ Fleet configured with Elastic Agent
✅ Windows endpoint enrolled
✅ Logs visible in Kibana Discover

This educational project replicates a real-world enterprise SIEM workflow using open-source Elastic technology.





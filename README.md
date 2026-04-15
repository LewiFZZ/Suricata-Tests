# 🛡️ Suricata Custom Rules — Network Traffic Detection Lab

A practical lab demonstrating custom Suricata IDS rule creation and validation in a virtualized environment.

---

## 📋 Overview

This repository contains a set of custom Suricata rules designed to detect common network traffic types targeting the internal network (`$HOME_NET`). Each rule was tested in a controlled VirtualBox environment using a bridged network adapter, with traffic generated from a host machine to a Kali Linux guest VM.

> **Note:** Some screenshots contain alerts classified as `SURICATA Ethertype unknown`. These are known false positives caused by STP/RSTP Layer 2 BPDUs visible when operating in bridged network mode, and are unrelated to the custom rules being tested.

---

## 🧪 Lab Environment

| Component | Details |
|---|---|
| **Host OS** | Linux Mint 22.3 x86_64 |
| **Guest OS** | Kali Linux (VirtualBox) |
| **Suricata Version** | 8.0.4 RELEASE |
| **Network Mode** | Bridged Adapter |
| **Rule Source** | Custom — `/var/lib/suricata/rules/lcandeias.rules` |

---

## 📁 Repository Structure

```text
├── screenshots/
│   ├── 1-instalar_suricata.jpg
│   ├── 2-interfaz_de_red.jpg
│   ├── 3-archivo_rules.jpg
│   ├── 4-verificacion_reglas.jpg
│   ├── 5-status_suricata.jpg
│   ├── 6-suricata-update.jpg
│   ├── 7-prueba_icmp.jpg
│   ├── 8-prueba_ssh.jpg
│   ├── 9-prueba_ftp.jpg
│   └── 10-prueba_http.jpg
└── lcandeias.rules
```

---

## 📜 Custom Rules

```suricata
# Rule 1 -- ICMP Detection
alert icmp any any -> $HOME_NET any (msg:"ICMP detectado: ping hacia menguidanos"; sid:1000001;)

# Rule 2 -- FTP Traffic Detection (port 21)
alert tcp any any -> $HOME_NET 21 (msg:"FTP detectado: tráfico hacia puerto 21"; sid:1000002;)

# Rule 3 -- SSH Traffic Detection (port 22)
alert tcp any any -> $HOME_NET 22 (msg:"SSH detectado: tráfico hacia puerto 22"; sid:1000003;)

# Rule 4 -- HTTP Traffic Detection (port 80)
alert tcp any any -> $HOME_NET 80 (msg:"HTTP detectado: tráfico hacia puerto 80"; sid:1000004;)
```

---

## 🔬 Tests Performed

Each rule was triggered by generating traffic from the host machine toward the guest VM's IP address.

| Rule | Protocol | Port | Test Method | Result |
|---|---|---|---|---|
| `1000001` | ICMP | — | `ping <VM_IP>` | ✅ Detected |
| `1000002` | TCP | 21 | `nc <VM_IP> 21` | ✅ Detected |
| `1000003` | TCP | 22 | `ssh <VM_IP>` | ✅ Detected |
| `1000004` | TCP | 80 | `curl http://<VM_IP>` | ✅ Detected |

Alerts were verified in real time using:

```bash
sudo tail -f /var/log/suricata/fast.log
```

---

## ✅ Rule Validation

Configuration and rule syntax were verified before deployment:

```bash
sudo suricata -T -c /etc/suricata/suricata.yaml
```

Output confirmed:

```text
Configuration provided was successfully loaded.
```

---

## 📦 Ruleset Update

Community rules were updated using the official Suricata update tool:

```bash
sudo suricata-update
```

A total of **65,499 rules** were loaded from the Emerging Threats Open ruleset alongside the custom ruleset.

---

## 📄 License

This project is intended for educational purposes only. Use responsibly in controlled lab environments.
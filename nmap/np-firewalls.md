

---

### 🔥 **Firewall and IDS/IPS Basics**

#### 🔒 Firewalls:

- **Purpose**: Block/allow connections based on rules (ports, IPs, protocols).
    
- **Packet behavior**:
    
    - **Dropped**: Silently ignored (no reply).
        
    - **Rejected**: Response sent (ICMP error or TCP RST).
        

#### 🧠 IDS vs. IPS:

- **IDS (Intrusion Detection System)**: Monitors and alerts.
    
- **IPS (Intrusion Prevention System)**: Monitors + actively blocks.
    

---

### 🔍 **Techniques for Evasion & Detection with Nmap**

#### 1. **SYN Scan** (`-sS`)

- Stealthy.
    
- Can be blocked by firewalls.
    
- Open port: SYN/ACK.
    
- Closed port: RST.
    

#### 2. **ACK Scan** (`-sA`)

- Used to **bypass stateful firewalls**.
    
- Determines if a port is filtered or unfiltered.
    
- Doesn’t show open/closed state.
    
- If RST → port is unfiltered.
    
- No response → filtered.
    

#### 3. **Decoy Scan** (`-D RND:5`)

- Uses fake IPs to confuse IDS/IPS.
    
- Your real IP is mixed with random decoys.
    
- Needs decoys to be **live hosts**.
    
- Helps **evade detection** by masking real source.
    

#### 4. **Custom Source IP Scan** (`-S`, `-e`)

- Spoofs source IP.
    
- Useful to **test access** from different subnets.
    
- Must use appropriate network interface (`-e`).
    

#### 5. **Using DNS (Port 53) to Bypass**

- Many firewalls allow DNS (UDP/TCP 53) traffic.
    
- Use `--source-port 53` in Nmap.
    
- Example: bypass firewall filtering port 50000 using TCP source port 53.
    

---

### 🔬 **Detecting IDS/IPS**

- Aggressive scans from 1 IP → check if that IP gets blocked.
    
- Use **multiple VPS IPs** to test.
    
- If blocked, slow down and **go stealthy**.
    
- IDS: logs and alerts admin.
    
- IPS: auto-blocks detected threats.
    

---

### 🧪 **Practical Lab Takeaways**

- **Compare SYN vs. ACK scan** to detect firewall behavior.
    
- Use `--packet-trace` to observe packet flags (SA, RST, no response).
    
- **Netcat** can be used to test direct connections using `--source-port`.
    

---

### 🧠 Tips for Real-World Pentesting or CTFs

- Start passive: `-sA`, `-Pn`, `-n`, `--disable-arp-ping`.
    
- Use decoys early if unsure about IPS presence.
    
- Use `--source-port 53` if you suspect firewalls allow DNS.
    
- Watch how the network responds: **RST = port reachable**, **no response = likely filtered**.
    

---

Would you like a **cheat sheet**, **flashcards**, or **practice scenario** for these techniques?
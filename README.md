.
## 🏗️ Project Overview: Multi-Router OSPF Network

This project demonstrates a multi-area (Area 0) OSPF configuration across four Cisco routers. It connects four distinct LAN segments (192.168.x.x) through a serial/gigabit backbone (10.0.0.x, 30.0.0.x, and 20.0.0.x).

---

## 🛠️ Step 1: Router Interface & Hostname Configuration

Run these commands on each router's Command Line Interface (CLI).

### **Router 1 (R1)**

```bash
Router> enable
Router# configure terminal
Router(config)# hostname R1
R1(config)# interface Gig0/0
R1(config-if)# ip address 192.168.1.10 255.255.255.0
R1(config-if)# no shutdown
R1(config-if)# exit
R1(config)# interface Gig0/1
R1(config-if)# ip address 10.0.0.1 255.255.255.0
R1(config-if)# no shutdown

```

### **Router 2 (R2)**

```bash
Router(config)# hostname R2
R2(config)# interface Gig0/0
R2(config-if)# ip address 192.168.2.10 255.255.255.0
R2(config-if)# no shutdown
R2(config)# interface Gig0/1
R2(config-if)# ip address 10.0.0.2 255.255.255.0
R2(config-if)# no shutdown
R2(config)# interface Gig0/2
R2(config-if)# ip address 30.0.0.3 255.255.255.0
R2(config-if)# no shutdown

```

### **Router 3 (R3)**

```bash
Router(config)# hostname R3
R3(config)# interface Gig0/0
R3(config-if)# ip address 192.168.3.10 255.255.255.0
R3(config-if)# no shutdown
R3(config)# interface Gig0/2
R3(config-if)# ip address 30.0.0.4 255.255.255.0
R3(config-if)# no shutdown
R3(config)# interface Gig0/1
R3(config-if)# ip address 20.0.0.5 255.255.255.0
R3(config-if)# no shutdown

```

### **Router 4 (R4)**

```bash
Router(config)# hostname R4
R4(config)# interface Gig0/0
R4(config-if)# ip address 192.168.4.10 255.255.255.0
R4(config-if)# no shutdown
R4(config)# interface Gig0/1
R4(config-if)# ip address 20.0.0.6 255.255.255.0
R4(config-if)# no shutdown

```

---

## 🌐 Step 2: OSPF Routing Configuration (Process ID 1)

We will use **Wildcard Masks** (0.0.0.255 for a /24 network) to advertise the interfaces.

| Router | Router ID | Networks to Advertise |
| --- | --- | --- |
| **R1** | 1.1.1.1 | 192.168.1.0, 10.0.0.0 |
| **R2** | 2.2.2.2 | 192.168.2.0, 10.0.0.0, 30.0.0.0 |
| **R3** | 3.3.3.3 | 192.168.3.0, 30.0.0.0, 20.0.0.0 |
| **R4** | 4.4.4.4 | 192.168.4.0, 20.0.0.0 |

**Example CLI for R2:**

```bash
R2(config)# router ospf 1
R2(config-router)# router-id 2.2.2.2
R2(config-router)# network 192.168.2.0 0.0.0.255 area 0
|R2(config-router)# network 10.0.0.0 0.0.0.255 area 0
R2(config-router)# network 30.0.0.0 0.0.0.255 area 0

```

*(Apply similar logic to R1, R3, and R4 using their respective IPs and IDs.)*

---

## 💻 Step 3: End Device Setup (Static IP)

Configure the PCs and Laptops via their **Desktop > IP Configuration** tab:

* **PC0 (LAN 1):** IP: `192.168.1.1` | Subnet: `255.255.255.0` | Gateway: `192.168.1.10`
* **Server0 (LAN 2):** IP: `192.168.2.1` | Subnet: `255.255.255.0` | Gateway: `192.168.2.10`
* **Server1 (LAN 3):** IP: `192.168.3.1` | Subnet: `255.255.255.0` | Gateway: `192.168.3.10`
* **Laptop2 (LAN 4):** IP: `192.168.4.1` | Subnet: `255.255.255.0` | Gateway: `192.168.4.10`

---
.

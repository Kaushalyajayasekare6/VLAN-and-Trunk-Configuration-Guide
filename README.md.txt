# VLAN and Trunk Configuration Guide

## Overview
This guide provides step-by-step instructions to configure VLANs and trunking between two switches (**sw1** and **sw2**). By following this setup, you'll enable proper VLAN segmentation and inter-switch communication via trunking.

---

## 🔧 Configuration Steps

### 🏗 Setting Up **sw1**

1. **Enter Configuration Mode**
   ```bash
   Switch> enable
   Switch# configure terminal
   ```
2. **Set the Switch Hostname**
   ```bash
   Switch(config)# hostname sw1
   ```
3. **Create VLANs and Assign Names**
   ```bash
   sw1(config)# vlan 10
   sw1(config-vlan)# name AA
   sw1(config-vlan)# exit

   sw1(config)# vlan 20
   sw1(config-vlan)# name BB
   sw1(config-vlan)# exit
   ```
4. **Assign VLANs to Interfaces**
   ```bash
   sw1(config)# interface range fa0/1-2
   sw1(config-if-range)# switchport mode access
   sw1(config-if-range)# switchport access vlan 10
   sw1(config-if-range)# exit

   sw1(config)# interface range fa0/3-4
   sw1(config-if-range)# switchport mode access
   sw1(config-if-range)# switchport access vlan 20
   sw1(config-if-range)# exit
   ```
5. **Configure Native VLAN & Trunking**
   ```bash
   sw1(config)# vlan 99
   sw1(config-vlan)# name native
   sw1(config-vlan)# exit

   sw1(config)# interface fa0/5
   sw1(config-if)# switchport mode trunk
   sw1(config-if)# switchport trunk native vlan 99
   sw1(config-if)# switchport trunk allowed vlan 10,20
   sw1(config-if)# exit
   ```
6. **Save Configuration**
   ```bash
   sw1# write memory
   ```

---

### 🏗 Setting Up **sw2**

1. **Enter Configuration Mode**
   ```bash
   Switch> enable
   Switch# configure terminal
   ```
2. **Set the Switch Hostname**
   ```bash
   Switch(config)# hostname sw2
   ```
3. **Create VLANs and Assign Names**
   ```bash
   sw2(config)# vlan 10
   sw2(config-vlan)# name AA
   sw2(config-vlan)# exit

   sw2(config)# vlan 20
   sw2(config-vlan)# name BB
   sw2(config-vlan)# exit

   sw2(config)# vlan 99
   sw2(config-vlan)# name native
   sw2(config-vlan)# exit
   ```
4. **Assign VLANs to Interfaces**
   ```bash
   sw2(config)# interface fa0/1
   sw2(config-if)# switchport mode access
   sw2(config-if)# switchport access vlan 10
   sw2(config-if)# exit

   sw2(config)# interface fa0/2
   sw2(config-if)# switchport mode access
   sw2(config-if)# switchport access vlan 20
   sw2(config-if)# exit
   ```
5. **Configure Trunking**
   ```bash
   sw2(config)# interface fa0/3
   sw2(config-if)# switchport mode trunk
   sw2(config-if)# switchport trunk native vlan 99
   sw2(config-if)# switchport trunk allowed vlan 10,20
   sw2(config-if)# exit
   ```
6. **Save Configuration**
   ```bash
   sw2# write memory
   ```

---

## 🎯 Verification Commands

🔹 **Check VLAN Configuration:**
```bash
sw1# show vlan brief
sw2# show vlan brief
```

🔹 **Check Trunking Status:**
```bash
sw1# show interfaces trunk
sw2# show interfaces trunk
```

🔹 **Check Spanning Tree Issues (if any):**
```bash
sw1# show spanning-tree
sw2# show spanning-tree
```

---

## 🚀 Final Thoughts
This guide ensures seamless VLAN configuration and trunking between switches. If you encounter errors like `Native VLAN mismatch` or `BPDU inconsistency`, verify that both switches have the same native VLAN and trunk settings.

Happy Networking! 🌍⚡


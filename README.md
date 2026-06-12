

## Project Overview

This project implements a simplified Layer-2 VPN similar to ZeroTier. The system consists of a Virtual Switch (VSwitch) and multiple Virtual Ports (VPorts) that enable computers located across the Internet to communicate as if they were connected to the same Local Area Network (LAN).

The VSwitch performs Ethernet frame forwarding based on MAC addresses, while VPorts act as virtual switch ports that connect the Linux kernel through TAP devices and communicate with the VSwitch using UDP sockets.

---

## Objectives

* Understand Layer-2 switching concepts.
* Implement MAC address learning and forwarding.
* Create a virtual Ethernet network over the Internet.
* Establish secure communication between remote hosts using TAP devices.
* Simulate the functionality of a physical Ethernet switch.

---

## System Architecture

The project consists of:

### VSwitch (Server)

* Maintains a MAC Address Table.
* Learns source MAC addresses dynamically.
* Forwards Ethernet frames based on destination MAC addresses.
* Broadcasts frames when destination MAC is unknown.

### VPort (Client)

* Creates and manages a TAP device.
* Reads Ethernet frames from TAP.
* Encapsulates Ethernet frames into UDP packets.
* Sends packets to VSwitch.
* Receives packets from VSwitch and writes them back to TAP.

Communication Flow:

Linux Kernel ↔ TAP Device ↔ UDP Socket ↔ VSwitch ↔ UDP Socket ↔ TAP Device ↔ Linux Kernel

---

## Technologies Used

* Python 3 (VSwitch)
* C Programming (VPort)
* Linux TAP Devices
* UDP Sockets
* Ethernet Frames
* Linux Networking Utilities

---

## Build Procedure

Compile the client application:

```bash
make
```

This generates the executable:

```bash
./vport
```

---

## Deployment Steps

### Step 1: Start VSwitch

On the server machine:

```bash
python3 vswitch.py 9000
```

---

### Step 2: Configure Client 1

Start VPort:

```bash
sudo ./vport SERVER_IP 9000
```

Configure TAP Interface:

```bash
sudo ip addr add 10.1.1.101/24 dev tapyuan
sudo ip link set tapyuan up
```

---

### Step 3: Configure Client 2

Start VPort:

```bash
sudo ./vport SERVER_IP 9000
```

Configure TAP Interface:

```bash
sudo ip addr add 10.1.1.102/24 dev tapyuan
sudo ip link set tapyuan up
```

---

## Testing

Verify connectivity by sending ICMP packets between clients.

From Client 1:

```bash
ping 10.1.1.102
```

From Client 2:

```bash
ping 10.1.1.101
```

Successful ping replies confirm proper MAC learning, frame forwarding, and Layer-2 VPN functionality.

---

## MAC Learning Mechanism

1. VSwitch receives an Ethernet frame.
2. Source MAC address is learned and stored.
3. Destination MAC is searched in MAC table.
4. If found, frame is forwarded to corresponding VPort.
5. If not found, frame is broadcast to all connected VPorts except sender.
6. Reply packets update the MAC table dynamically.

---

## Results

* Successfully created a virtual Layer-2 network over UDP.
* Implemented MAC address learning and switching.
* Achieved communication between geographically separated hosts.
* Verified connectivity using ICMP ping tests.
* Demonstrated the working principle behind VPN solutions such as ZeroTier.

---

## Conclusion

The project successfully implemented a simplified Layer-2 VPN system that emulates the functionality of an Ethernet switch over the Internet. Using TAP devices, UDP sockets, MAC address learning, and Ethernet frame forwarding, multiple remote systems were connected into a single virtual LAN. This project provided practical experience in network virtualization, switching mechanisms, and VPN technologies.

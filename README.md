# Net_Practice

> **42 Project #10** — Configuring small networks, making devices talk, applying TCP/IP concepts

This repo is mainly about understanding how networks work through 10 practical exercises. Recent update focused on UI improvements, better documentation, and overall cleanup.

---

## What's Inside

- [Basics](#basics) — Networks, TCP/IP, addressing
- [Subnetting](#subnetting) — IPv4, masks, CIDR notation
- [Devices](#devices) — Switches, routers, routing tables
- [Exercises](#exercises) — Walkthroughs for all 10 levels
- [Resources](#resources) — Helpful links and references

---

## Basics

### Networks
A network is just devices that can send/receive data between each other. 

- **Public** (Internet) — Anyone can connect, no real access control
- **Private** (Home/Office) — Restricted, safer, controlled

### TCP/IP
- **TCP** — Breaks data into packets, sends them, rebuilds them, ensures nothing gets lost
- **IP** — Addresses for devices on the network (we use IPv4 here)

They're different protocols that work together.

---

## Subnetting

### IPv4 Structure
- 32 bits split into 4 blocks (0-255 each)
- has two parts: **network** (which network) + **host** (which device)
- **subnet mask** tells you which bits belong to the network part

### Example
```
IP:    192.168.1.10
Mask:  255.255.255.0  (/24 in CIDR)
Range: 192.168.1.1 — 192.168.1.254
```

**Reserved IPs:**
- First in range → Subnet identifier
- Last in range → Broadcast

### Private IP Ranges (Don't Use Outside These)
- `10.0.0.0 — 10.255.255.255` (class A)
- `172.16.0.0 — 172.31.255.255` (class B)
- `192.168.0.0 — 192.168.255.255` (class C)
- `127.0.0.0 — 127.255.255.255` (loopback)

### CIDR Cheat Sheet
See `/configs/subnet_table.md` for full table

| CIDR | Mask | Usable IPs |
|------|------|------------|
| /30 | 255.255.255.252 | 2 |
| /28 | 255.255.255.240 | 14 |
| /27 | 255.255.255.224 | 30 |
| /26 | 255.255.255.192 | 62 |
| /25 | 255.255.255.128 | 126 |
| /24 | 255.255.255.0 | 254 |

---

## Devices

### Switch
Distributes packets between devices in the **same** network (LAN). Can't reach outside networks.

### Router
Connects **multiple** networks together. Each interface belongs to a different network. Ranges can't overlap.

### Routing Table
Tells routers where to send packets.

| Destination | Next Hop |
|-------------|----------|
| Where you're sending | Next router's IP |

- Use `default` or `0.0.0.0/0` to catch everything
- Next hop = IP of the next router interface on the path

---

## Exercises

<details>
<summary><b>Level 1</b> — Basic communication</summary>

![level 1](./images/level%2001.png)

Simple point-to-point connections. Just make sure both devices are in the same network by matching their network portions based on the subnet mask.

</details>

<details>
<summary><b>Level 2</b> — Matching masks</summary>

![level 2](./images/level%2002.png)

Both devices need the **same mask**. Can't use reserved IPs (network/broadcast addresses).

Key: Calculate the network range from one device's IP and mask, then assign the other device an IP in the same range.

</details>

<details>
<summary><b>Level 3</b> — Introducing switches</summary>

![level 3](./images/level%2003.png)

All 3 clients connect through a switch → Same network, same mask.

Calculate the network range from the given IPs/masks and make sure all devices fit in the same subnet.

</details>

<details>
<summary><b>Level 4</b> — Router with 3 interfaces</summary>

![level 4](./images/level%2004.png)

Router has 3 interfaces, **no overlapping ranges** allowed.

Look at the existing interface ranges, find the gap between them, then choose a mask that fits the client's IP in that gap without overlapping.

</details>

<details>
<summary><b>Level 5</b> — Routing tables</summary>

![level 5](./images/level%2005.png)

First routing table exercise. Clients need routes to reach each other through the router.

**Approach:**
- Match each client's network to its router interface
- Routing table: Destination can be `default` or the specific network you're trying to reach
- Next hop = The router interface IP on your side of the connection

</details>

<details>
<summary><b>Level 6</b> — Connecting to internet</summary>

![level 6](./images/level%2006.png)

Client needs to reach the internet through a router.

**Approach:**
- Client & router interface: Same network
- Client route: Destination `default`, next hop = Router's interface IP
- Internet route: Destination = Client's IP/mask, next hop = Router's other interface
- Router route: Destination `default` (doesn't need to be specific)

</details>

<details>
<summary><b>Level 7</b> — Avoiding overlaps</summary>

![level 7](./images/level%2007.png)

3 networks, **no overlaps** between router interfaces.

**Approach:**
- Identify fixed IPs on the routers
- Choose masks that create non-overlapping subnets
- Assign IPs within those ranges
- Configure routing: Clients point to their router interfaces, routers point to each other

</details>

<details>
<summary><b>Level 8</b> — Internet from 2 hosts</summary>

![level 8](./images/level%2008.png)

2 clients need internet access. Internet expects a specific IP range.

**Approach:**
- Check internet's routing table for the allowed range
- Split that range into non-overlapping subnets for the 3 networks (client C, client D, router-to-router)
- Make sure one router's next hop matches the other router's interface IP
- Clients route to `default`, next hop = Their router interface

</details>

<details>
<summary><b>Level 9</b> — Complex multi-network</summary>

![level 9](./images/level%2009.png)

4 networks, all concepts combined.

**Approach:**
- Identify all networks and their requirements
- Assign non-overlapping subnets with appropriate masks
- Configure all routing tables to allow cross-network communication
- Internet needs routes to the hosts that will access it directly

</details>

<details>
<summary><b>Level 10</b> — Single internet route</summary>

![level 10](./images/level%2010.png)

Internet has one route to the router — All networks must fit inside that range without overlapping.

**Approach:**
- Check internet's routing table for the single allowed range
- Divide that range into 4 non-overlapping subnets
- Configure all devices within their respective subnets
- Route everything through the main router to reach internet

</details>

---

## Resources

- [Networking Basics Playlist](https://www.youtube.com/watch?v=S7MNX_UD7vY&list=PLIhvC56v63IJVXv0GJcl9vO5Z6znCVb1P)
- [Subnet Calculator](https://www.calculator.net/ip-subnet-calculator.html)
- [Visual Subnet Calculator](http://www.davidc.net/sites/default/subnets/subnets.html)

---
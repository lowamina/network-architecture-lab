# Lab 01 — Discover Your Own Network

**Goal:** see your own device's addresses, your gateway, and who else is on your
network. Run this at home, then (read-only) on uni Wi-Fi to compare.

**Scope:** read-only commands on connections you're allowed to use. Nothing here
changes or attacks anything.

---

## 1. Find your own IP and gateway

**Windows (CMD):**
```cmd
ipconfig /all
```
**Linux / macOS:**
```bash
ip addr          # or: ifconfig
ip route         # shows your default gateway
```

Record:
- Your IPv4 address: __________ (e.g. `192.168.1.10`)
- Subnet mask: __________ (e.g. `255.255.255.0` = `/24`)
- Default gateway: __________ (e.g. `192.168.1.1`)

**Question:** Is your IP in the `192.168`, `172.16–31`, or `10.x` range? What does
that tell you about the network size?

---

## 2. See who else is on your network (ARP table)

```bash
arp -a
```

This lists devices your machine has recently talked to on the local subnet.

- At **home**, you'll usually see several devices (phones, TV, other laptops).
- On **uni Wi-Fi**, you'll often see **very few or none** — that's *client
  isolation* deliberately separating students from each other.

Count the devices:
- Home: __________
- Uni: __________

---

## 3. Find your public IP

Your public IP is what the internet sees — `ipconfig`/`ip` will **not** show it.

**Windows & Linux:**
```bash
curl ifconfig.me
```
or
```bash
nslookup myip.opendns.com resolver1.opendns.com
```

Record your public IP: __________

**Question:** If two devices in your home both run `curl ifconfig.me`, do they get
the same public IP? Why? (Hint: NAT.)

---

## 4. Trace the path out

```bash
tracert 8.8.8.8      # Windows
traceroute 8.8.8.8   # Linux/macOS
```

The first hop is your gateway. Count how many hops before you reach the internet.

- Home: __________ hops
- Uni: __________ hops (usually more — extra firewall/router layers)

---

## What you learned

- The difference between your **private** (local) and **public** (internet-facing) IP.
- How to read your subnet and gateway.
- That home networks are **flat and visible**, while enterprise networks are
  **segmented and isolated**.

➡️ Next: [Lab 02 — Public vs. Private IP & NAT](02-public-private-nat.md)

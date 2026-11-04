# Lab 02 — Public vs. Private IP & NAT

**Goal:** understand why many devices with private IPs can all reach the internet
through one (or a few) public IPs.

**Scope:** your own home network.

---

## Background

- **Private IPs** (`192.168.x`, `172.16–31.x`, `10.x`) are not routable on the
  internet. Routers on the internet drop them.
- **Public IPs** are globally unique and routable.
- **NAT (Network Address Translation)** lives in your router. It rewrites the
  source address of outgoing packets from your private IP to the router's public
  IP, and keeps a table so replies come back to the right device.

```
Laptop 192.168.1.10 ─┐
Phone  192.168.1.11 ─┼─► ROUTER ─► NAT ─► public IP 103.21.x.x ─► Internet
TV     192.168.1.12 ─┘
```

---

## 1. Prove multiple devices share one public IP

On two different devices on your home Wi-Fi:
```bash
curl ifconfig.me
```
Both should return the **same** public IP. That's NAT at work — one public IP,
many private devices.

---

## 2. Look at the NAT/DHCP table in your router

1. Open your router admin page (usually `http://192.168.1.1` or `192.168.0.1`).
2. Log in (credentials are often on a sticker on the router).
3. Find **DHCP Clients** / **Connected Devices** — this lists the private IPs
   your router handed out.
4. Find **NAT** / **Port Forwarding** settings to see how NAT is configured.

> ⚠️ Only log into **your own** router. Look around, but don't change settings
> unless you know how to undo them.

List the private IPs your router assigned:
- __________
- __________
- __________

---

## 3. CGNAT check (common in Sri Lanka)

Many local ISPs (Dialog, SLT, Mobitel) use **Carrier-Grade NAT**, so even your
"public" IP is shared with other customers.

Compare:
```bash
curl ifconfig.me
```
...with the WAN IP shown on your **router's status page**.

- If they **match** → you have a real public IP.
- If they **differ** (router shows a `100.64.x.x` or another private-looking
  address) → you're behind **CGNAT**: your ISP is doing a second layer of NAT.

Your result: __________

---

## What you learned

- Why private IPs can't be reached directly from the internet.
- How NAT multiplexes many devices onto one public IP.
- What CGNAT is and how to detect it.

➡️ Next: [Lab 03 — Home vs. Enterprise: Spot the Differences](03-home-vs-enterprise.md)

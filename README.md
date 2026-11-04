# Home vs. Enterprise Network Architecture — A Hands-On Lab

A beginner-friendly lab project that compares **home router systems** with
**enterprise/university router + access-point systems**, with a focus on how
**public and private IPv4 addressing, NAT, VLANs, and authentication** differ
between the two.

Every exercise in this repo is designed to be run on **equipment you own or are
explicitly authorised to test** — your own home router, your own devices, and
free browser-based labs. Nothing here targets networks you don't control.

---

## Why this project

Most people use two very different kinds of network every day without noticing:

- The **all-in-one box** at home that does everything.
- The **multi-layer system** at a university or company, built from separate
  specialised devices.

Understanding the difference explains a lot: why you get a `192.168.x.x` address
at home but a `10.x.x.x` address on campus, why you can see other devices at home
but not at uni, why you log in with a shared password at home but your own
credentials at uni, and why enterprise networks are so much harder to disrupt.

---

## The core idea in one picture

### Home network — one device does everything

```
Internet
   │  1 public IP (often shared via CGNAT)
┌──┴───────────────────────────┐
│   HOME ROUTER (all-in-one)    │
│   router + firewall + NAT     │
│   + DHCP + switch + Wi-Fi AP  │
└──┬───────────────────────────┘
   │  private subnet 192.168.1.0/24
   ├─ Laptop  192.168.1.10
   ├─ Phone   192.168.1.11
   └─ TV      192.168.1.12
```

### Enterprise / university — specialised layers

```
Internet
   │  a BLOCK of public IPs (e.g. a /28 or larger)
┌──┴──────────────┐
│  EDGE FIREWALL   │  NAT, filtering, IDS/IPS, logging
└──┬──────────────┘
┌──┴──────────────┐
│   CORE ROUTER    │  routes between internal subnets
└──┬──────────────┘
┌──┴──────────────┐
│ DISTRIBUTION /   │  Layer-3 switches, VLANs
│ ACCESS SWITCHES  │
└──┬──────────────┘
   ├────────────┬────────────┐
┌──┴──┐      ┌──┴──┐      ┌──┴──┐
│ AP  │      │ AP  │ ...  │ AP  │   many "thin" access points
└─────┘      └─────┘      └─────┘
   └──── all managed by a WLAN CONTROLLER + RADIUS (802.1X) ────┘
```

---

## Side-by-side comparison

| Aspect           | Home                         | University / Enterprise                        |
|------------------|------------------------------|------------------------------------------------|
| Hardware         | 1 all-in-one router          | Firewall + router + switches + APs + controller |
| Public IPv4      | 1 (often CGNAT)              | A block / pool                                 |
| Private range    | `192.168.x.0/24` (one subnet)| `10.x.x.x` (many VLANs)                        |
| Access points    | Built into the router        | Many "thin" APs run by a controller            |
| Login            | 1 shared Wi-Fi password      | Per-user 802.1X credentials (RADIUS)           |
| Segmentation     | None — flat network          | Heavy — VLAN isolation between groups          |
| Device visibility| You can see other devices    | Usually isolated — few/no peers visible        |
| Monitoring       | None                         | Firewall logs + wireless IDS/IPS (WIPS)        |

---

## Public vs. private IPv4 — quick reference

| Range                         | Size        | Typically used by          |
|-------------------------------|-------------|----------------------------|
| `10.0.0.0 – 10.255.255.255`   | ~16 million | Large orgs, universities   |
| `172.16.0.0 – 172.31.255.255` | ~1 million  | Medium networks            |
| `192.168.0.0 – 192.168.255.255`| ~65,000    | Home networks              |

**NAT** lets many private IPs share one or a few public IPs. Both home and
enterprise use it — just at wildly different scales.

---

## Labs

Work through them in order. Each is self-contained and runs on your own gear.

1. [Lab 01 — Discover Your Own Network](labs/01-discover-your-network.md)
2. [Lab 02 — Public vs. Private IP & NAT](labs/02-public-private-nat.md)
3. [Lab 03 — Home vs. Enterprise: Spot the Differences](labs/03-home-vs-enterprise.md)
4. [Lab 04 — Wireless Management Frames & PMF (on your own router)](labs/04-wireless-pmf.md)

---

## Ethics & scope

This project is about **understanding and defending** networks.

- Run commands and tests only on **your own** network/devices, or on labs that
  explicitly authorise it (e.g. TryHackMe, Hack The Box).
- Never scan, deauth, flood, or otherwise disrupt a network you don't own —
  including your university's. It's against acceptable-use policies and, in most
  countries (including Sri Lanka's Computer Crimes Act No. 24 of 2007), it's a
  criminal offence.
- The goal is to become the person who **secures** these systems.

---

## Tech used

- `ipconfig` / `ip` / `arp` / `traceroute` / `nslookup` (built into Windows & Linux)
- Wireshark (packet capture)
- Your home router's admin page
- Optional: a spare router with OpenWrt for deeper experiments

## License

MIT — see [LICENSE](LICENSE).

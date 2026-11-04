# Lab 03 — Home vs. Enterprise: Spot the Differences

**Goal:** use observations (read-only) to identify which architectural features a
network has, and explain *why*.

**Scope:** your home network, plus read-only observation of any network you're
authorised to use (e.g. uni Wi-Fi as a normal logged-in user).

---

## 1. Fill in the comparison from your own data

Using results from Labs 01–02:

| Observation                 | Home              | Uni / Enterprise  |
|-----------------------------|-------------------|-------------------|
| Your private IP range       |                   |                   |
| Subnet size (/24? /16?)     |                   |                   |
| Devices visible in `arp -a` |                   |                   |
| Hops to the internet        |                   |                   |
| Login type                  | shared password   | your own login    |

---

## 2. Explain each difference

Answer in your own words (great material for your README write-up):

1. **Why is the uni's private range `10.x` instead of `192.168.x`?**
   (Hint: how many devices must it address?)

2. **Why can you see other devices at home but not at uni?**
   (Hint: VLANs and client isolation.)

3. **Why does the uni use per-user logins instead of one shared password?**
   (Hint: accountability, 802.1X, RADIUS.)

4. **Why are there more hops to the internet on campus?**
   (Hint: firewall → core router → distribution → access layers.)

---

## 3. The "thin AP" concept

At home, the access point **is** your router. On campus, access points are
"thin" — they're just radios, and a central **WLAN controller** makes all the
decisions.

**Question:** When you walk from one building to another and stay connected
without re-entering your password, which component makes that possible? Why can't
a home setup do the same?

---

## 4. Diagram it yourself

Draw (on paper or in a tool like draw.io) the two architectures from memory:
- Home: one box, one subnet.
- Enterprise: firewall, core router, switches, APs, controller, RADIUS, VLANs.

Add both diagrams to your repo — they make the project much stronger.

---

## What you learned

- How to infer architecture from observable behaviour.
- The role of VLANs, thin APs, controllers, and 802.1X.
- Why enterprise design trades simplicity for scale, security, and accountability.

➡️ Next: [Lab 04 — Wireless Management Frames & PMF](04-wireless-pmf.md)

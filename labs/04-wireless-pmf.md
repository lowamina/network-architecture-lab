# Lab 04 — Wireless Management Frames & PMF (Defensive)

**Goal:** understand why older Wi-Fi is vulnerable to deauthentication, and see
how **Protected Management Frames (PMF)** / **WPA3** fix it.

> ⚠️ **Strict scope — read this first.**
> This lab is run **only against your own router and your own test device**, in
> your own home, with no one else relying on that Wi-Fi.
> Sending deauth frames at any network or device you don't own is a
> denial-of-service attack — against acceptable-use policies and illegal under
> Sri Lanka's Computer Crimes Act (and equivalent laws elsewhere). The point of
> this lab is to **see the defence work**, not to disrupt anyone.

**You'll need:** your own router (one that can toggle PMF/WPA3), a spare test
device you own, a Linux machine (Kali), and a Wi-Fi adapter that supports monitor
mode.

---

## The concept

Old Wi-Fi sent **management frames** (including "disconnect" / deauth messages)
**unencrypted and unauthenticated**. So a forged "disconnect" frame would be
believed, kicking a device off.

**PMF (802.11w)** cryptographically protects those frames. A forged deauth no
longer carries a valid key, so the client and AP **ignore it**. PMF is
**mandatory in WPA3** and available in WPA2.

---

## 1. Capture the handshake behaviour (observation only)

On your own network, put your adapter in monitor mode and watch the management
frames in **Wireshark** (filter: `wlan.fc.type == 0`). You'll see beacons,
association, and (if any) deauth frames. Just observe — no injection yet.

---

## 2. Baseline: PMF **disabled**

1. In your router admin page, set the Wi-Fi to **WPA2-Personal** with **PMF =
   Disabled** (sometimes "802.11w = off").
2. Connect your **test device**.
3. From Kali, run a short deauth burst **against your own test device/AP**:
   ```bash
   # OWN NETWORK ONLY — BSSID and client are your own devices
   sudo aireplay-ng --deauth 5 -a <your-AP-BSSID> -c <your-test-device-MAC> wlan0mon
   ```
4. Observe: the test device briefly drops and reconnects.

**Record:** did the device disconnect? __________

---

## 3. The fix: PMF **enabled** (or WPA3)

1. In the router, set **PMF = Required** (or switch the SSID to **WPA3**).
2. Reconnect the test device.
3. Run the **exact same** command as step 2.
4. Observe: the deauth frames are sent, but the device **stays connected** — they
   fail the integrity check and are ignored.

**Record:** did the device disconnect this time? __________

---

## 4. Compare in Wireshark

Capture both runs and compare the deauth frames. With PMF on, note the
Management Frame Protection fields. Explain in your write-up *why* the second set
of frames has no effect.

---

## What you learned

- Why unprotected management frames are a Wi-Fi weakness.
- How PMF/WPA3 defeats deauth attacks.
- Why enterprise networks (which use PMF + wireless IDS) shrug these off — tying
  back to Lab 03.

## Takeaway for your write-up

The valuable security insight isn't "deauth exists" — it's **"here's the
vulnerability, here's the defence, and here's how to verify the defence works."**
That framing is what employers and lecturers want to see.

---

🎓 **Project complete.** Add your diagrams, your recorded results, and a short
reflection to the README, and you've got a solid portfolio piece.

# Keeping the Tablet From Updating Itself

A companion to the [main guide](README.md), for **PLTN-RB1VO-2** tablets
(FCC ID 2AA3N-RB1VO2) — and, in substance, for the other original-Bike
tablets too. No specific bike or owner is covered.

**Why you'd want this:** Peloton software updates are the main thing that
can break a sideloaded setup (see the main guide): they can remove
sideloaded apps, reset the default launcher and permissions, and — in the
worst case — change how Developer Options / ADB behave. If your setup
works, freezing the software where it is keeps it working.

**The honest headline first:** Peloton offers **no opt-out**. There is no
"automatic updates: off" switch for system software — the *System updates*
screen under *Settings → Device Settings → About Tablet* only checks for
and installs updates ([Peloton's own instructions](https://support.onepeloton.com/s/article/Peloton-Software-How-to-Update-Your-Device)),
and updates otherwise download and install on Peloton's schedule,
typically while the bike sits idle or sleeps. Everything below is a
workaround layered *around* that, and each option is rated for how well it
actually works.

---

## How an update reaches the tablet

Understanding the delivery path tells you where it can be cut:

1. The tablet periodically **checks** Peloton's servers over HTTPS
   (TCP 443; Peloton's [firewall documentation](https://support.onepeloton.com/s/article/6428775274900-Bike-and-BikePlus-Corporate-Firewalls-Whitelist-Addresses)
   lists outbound TCP 80/443 and UDP 123/NTP as the bike's network needs).
2. The update package is **downloaded** from Peloton's domains and their
   CDNs — the same official whitelist names Peloton's API, content and
   update traffic: `*.onepeloton.com`, `*.pelotoncycle.com`, plus delivery
   infrastructure such as `*.s3.amazonaws.com` and `*.akamaihd.net`.
3. It **installs** automatically, usually unattended.

There is no published, update-only domain you can block in isolation:
checks, downloads, login, classes and the app itself share domains and
CDN hostnames. That single fact drives the effectiveness ratings below.

---

## The options

### Option 1 — Keep the tablet off the internet entirely

Forget the Wi-Fi network on the tablet (*Settings → Network → Wi-Fi →
your network → Forget*), don't plug in Ethernet, and leave it that way.

- **Effectiveness: complete.** No network path, no check, no download, no
  install. This is the only option that is not a partial measure.
- **Costs:** The tablet loses everything online — no streaming apps, no
  MyWhoosh/Zwift *on the tablet itself*, no Peloton login. What still
  works: **Just Ride** with live metrics (it never needed a connection),
  apps that run fully offline, SmartSpin2k in headless mode, and riding
  with a second device (phone/tablet) for the actual training app.
- **Undo:** Rejoin Wi-Fi — and expect it to update promptly once it can.

### Option 2 — Router: block internet access for the tablet only

Keep the tablet on your LAN, but deny it the WAN: most routers and ISP
apps can "pause" a device or block its internet access by MAC address
(access control / parental controls / per-device firewall rule). Reserve
the tablet a fixed IP in DHCP as well, so rules survive reboots.

- **Effectiveness: near-complete at home.** The tablet can't reach
  Peloton's servers at all, while your local network still can — so
  **Wi-Fi ADB from OpenPelo keeps working** (it's LAN-only traffic), and
  so does local streaming (e.g. Moonlight from a PC on the same network).
- **Costs:** Same online losses as Option 1 *for the tablet*; everything
  else in the house is unaffected.
- **Gotchas:**
  - Block **both** interfaces: the tablet has separate Wi-Fi and Ethernet
    MAC addresses (both are shown in *About Tablet → Status*).
  - Check the Wi-Fi network's privacy setting on the tablet: if it uses a
    **randomized MAC** for your SSID, a MAC rule may silently stop
    matching. Set that network to use the device MAC, or key the rule to
    the reserved IP as well.
  - The block only exists on *your* network. If the tablet is ever joined
    to another network (or someone re-joins it to yours with rules keyed
    to a different identifier), it can update.
- **Undo:** Remove the rule/pause.

### Option 3 — Router/DNS: block Peloton's domains for the tablet

Run DNS filtering (e.g. [Pi-hole](https://docs.pi-hole.net/group_management/example/)
with the tablet in its own group, or a NextDNS profile applied only to
the bike) and block Peloton's domains for that device only:
`onepeloton.com` and `pelotoncycle.com` (as whole-domain/wildcard rules).

- **Effectiveness: partial — a speed bump, not a wall.**
  - It stops the update *check* resolving — as long as DNS is the path
    used. It does nothing if a component ever connects by hardcoded IP or
    its own encrypted DNS (no public evidence Peloton does this today,
    but nothing stops a future build).
  - Blocking the domains also blocks **login, profile sync and the
    Peloton app itself** for that tablet — the update endpoints can't be
    separated from the service endpoints. If you also block the CDN names
    (`s3.amazonaws.com`, `akamaihd.net`) you'll break large parts of the
    wider internet for that device, since half the web is served from
    them — and the tablet's *other* apps may use them too.
  - Peloton can add or move endpoints at any time; a static list rots.
- **Right way to use it:** as a *sensor* as much as a block. Put the
  tablet in its own Pi-hole group, log its queries, then tap **Check for
  updates** on the tablet and watch which hosts it actually contacts —
  that's your real, current endpoint list, instead of a guessed one from
  the internet.
- **Undo:** Remove the domain rules / group assignment.

### Option 4 — ADB: disable the updater on the tablet

Android's package manager can disable a system component for the user
without root: `pm disable-user --user 0 <package>`. OpenPelo itself uses
exactly this mechanism on a Peloton package (it disables
`com.onepeloton.systempluginui` for its Netflix tool), so the mechanism is
proven on this tablet. The catch: **the exact package name of Peloton's
update component is not publicly documented**, so you find it on your own
tablet rather than trusting a name from a forum:

```text
adb shell pm list packages          :: full list; save it to a file first
```

Look for names containing `ota`, `fota`, `update`, or `upgrade`, and
Peloton's own `com.onepeloton.*` services. Then, one at a time:

```text
adb shell pm disable-user --user 0 <candidate-package>
adb shell pm list packages -d       :: confirms what is disabled
```

After each change, open *Settings → Device Settings → About Tablet →
System updates* on the tablet: the right component disabled makes that
screen fail, hang, or report it can't check — that's your confirmation.
Write down exactly what you disabled.

- **Effectiveness: high, if you find the right component** — the thing
  that checks/downloads/installs is switched off on the device itself, so
  it works on any network. Lower than Options 1–2 only because a wrong
  guess does nothing, and a *future* component (already-downloaded update,
  a second watchdog service) can still exist.
- **Risks:** Disabling the wrong package can break Peloton features or,
  worst case, the boot — which is why: one package at a time, test in
  between, keep your list, and know that a **factory reset restores
  everything** (it also wipes your sideloaded apps — the recovery is
  re-running the main guide's OpenPelo setup). Never disable the Peloton
  app/launcher itself unless you have already installed another launcher.
- **Undo:** `adb shell pm enable <package>` for each one.
- **Note:** An update that has *already downloaded* may install before
  you do this. Check the System updates screen first; if one is staged
  and pending a reboot, deal with that before relying on this option.

### Option 5 — On-device network restrictions (metered Wi-Fi, background-data limits)

Android generally won't auto-download system updates over a connection
marked **metered**: in the tablet's Wi-Fi settings for your network, set
*Metered → Treat as metered* (wording varies by build). Per-app
background-data restrictions live under *Settings → Apps → (app) → Mobile
data & Wi-Fi* where the build exposes them.

- **Effectiveness: low–moderate, and best treated as a delay.** These
  are hints Android gives its own updater; a vendor updater can ignore
  metered status, and restrictions reset if the network is re-added.
  Useful as an extra layer, never as the only one.
- **Undo:** Set the network back to unmetered/automatic.

### Option 6 — On-device firewall app (NetGuard and similar)

[NetGuard](https://github.com/M66B/NetGuard) is a no-root firewall that
uses Android's VPN service to allow/deny internet per app, including
system apps. Install it via OpenPelo/Aurora like any other app (main
guide §3), show system apps in its list, and deny network access to the
Peloton app/updater components while allowing your own apps.

- **Effectiveness: moderate–high while it's running.** It travels with
  the tablet (works on any network, unlike Options 2–3) and is more
  selective than domain blocking.
- **Caveats:** Only one app can hold Android's VPN slot, so this excludes
  running a real VPN alongside; if NetGuard is stopped, killed, or
  removed (e.g. by an update that *does* get through, or a factory
  reset), the protection is gone; per-app rules need the same care as
  Option 4 about which component actually fetches updates.
- **Undo:** Allow the apps again, or uninstall NetGuard.

### Option 7 — Tablet Private DNS to a resolver you control

*Settings → Network → Private DNS* (Android 9+) lets you point the
tablet's encrypted DNS at a filtering resolver you configure (e.g. a
NextDNS profile with Peloton's domains blocked) instead of your LAN's.

- **Effectiveness: same ceiling as Option 3** (it's the same DNS-layer
  block), with one advantage — it follows the tablet onto other networks —
  and the same weaknesses: it breaks Peloton login/app use on the tablet,
  and does nothing against non-DNS connections.
- **Undo:** Set Private DNS back to Automatic/Off.

---

## Side-by-side

| Option | Stops update check | Stops download/install | Tablet keeps internet for your apps | Works off your network | Effort | Reversible |
|---|---|---|---|---|---|---|
| 1. No internet at all | Yes | Yes | No | Yes (no network anywhere) | Trivial | Rejoin Wi-Fi |
| 2. Router WAN block (this device) | Yes | Yes | No (LAN only) | No | Low | Remove rule |
| 3. DNS domain block (this device) | Mostly | Mostly | Partly — Peloton domains dead, others fine | No | Medium | Remove rules |
| 4. ADB-disable the updater | Yes (right component) | Yes | Yes | Yes | Medium | `pm enable` |
| 5. Metered Wi-Fi / data limits | Delays | Delays | Yes | No | Trivial | Toggle off |
| 6. Firewall app (NetGuard) | Yes (while running) | Yes | Yes, per-app | Yes | Medium | Allow/uninstall |
| 7. Private DNS filtering | Mostly | Mostly | Partly | Yes | Low | Set to Automatic |

## Recommended combinations

- **"The tablet is a bike computer, nothing more":** Option 1 or 2, and
  do everything online on a second device. Simplest, strongest, nothing
  to maintain. Option 2 if you still want Wi-Fi ADB / local streaming.
- **"I ride MyWhoosh etc. on the tablet, but Peloton's software stays
  frozen":** Option 4 (ADB-disable the updater) **+** Option 6 or 7 as a
  second layer, with Option 3's logging trick used once to learn what
  your tablet actually contacts. Accept that this is defense-in-depth
  among partial measures — verify monthly that the build number in
  *About Tablet* hasn't changed.
- **"Minimum effort, some protection":** Option 5 + Option 3, knowing
  both are delays rather than guarantees.

## What doesn't work

- **Looking for an auto-update toggle.** There isn't one for system
  software on this tablet.
- **Dismissing or postponing the update prompt.** It defers, then
  installs anyway, typically unattended.
- **Never tapping "Check for updates."** The check is automatic and
  periodic; manual checks are not the trigger.
- **Blocking only a "download" URL copied from a forum.** Endpoints
  change, downloads ride shared CDNs, and an unverified list is how you
  break login while still updating. Discover your endpoints (Option 3)
  or cut the whole path (Options 1, 2, 4).

## The trade-offs of freezing — read before you commit

- **No security patches.** You are choosing to run an Android build that
  will only get older. On a single-purpose exercise tablet that never
  handles your email, banking, or password manager, that's a reasonable
  trade — don't reuse important accounts on it, and Options 1–2 shrink
  the exposure further.
- **Peloton can cut old software off server-side, and freezing can't
  stop that.** Precedent: software updates for the first-generation
  (Quartz, 2013–2016) tablets ended in 2019, and in June 2024 Peloton
  ended class access for them entirely — a server decision no router
  rule could have prevented. What survives any cutoff: local functions
  that don't need Peloton's servers (Just Ride's live metrics) and
  everything in the main guide that runs on your own apps and hardware.
- **Your own apps still need their own updates.** Freezing Peloton's
  software doesn't freeze Zwift/MyWhoosh/etc. — update those through
  Aurora Store/OpenPelo as usual; if one ever *requires* a newer Peloton
  build, that's a sign the frozen setup is reaching its end.
- **A factory reset undoes Options 4–6** (disabled packages, firewall
  app, DNS settings) — if you ever reset, redo them before the tablet
  sits online.
- **Verify, don't assume.** Once a month: *About Tablet* → note the
  build/Peloton OS version; glance at your router or Pi-hole log for the
  tablet. If the build changed, an update got through — reinstall your
  apps with OpenPelo (main guide §3) and tighten the layer that leaked.

---

## Sources

- Peloton Support — [How to update your device](https://support.onepeloton.com/s/article/Peloton-Software-How-to-Update-Your-Device) (update path; no opt-out offered)
- Peloton Support — [Bike and Bike+ corporate firewalls & whitelist addresses](https://support.onepeloton.com/s/article/6428775274900-Bike-and-BikePlus-Corporate-Firewalls-Whitelist-Addresses) (ports 80/443 TCP, 123 UDP; whitelist domains incl. `*.onepeloton.com`, `*.pelotoncycle.com`, `*.s3.amazonaws.com`, `*.akamaihd.net`)
- Peloton Business — [Network setup instructions](https://business.onepeloton.com/support/network-setup-instructions) (same allow-list, commercial setting)
- [OpenPelo — GitHub](https://github.com/doudar/Openpelo) (uses `pm disable-user --user 0` on Peloton system packages; Wi-Fi ADB workflow)
- [NetGuard — GitHub](https://github.com/M66B/NetGuard) (no-root, VPN-based per-app firewall; open source)
- Pi-hole documentation — [Group management examples](https://docs.pi-hole.net/group_management/example/) (per-client blocking groups)
- Android — [SDK Platform Tools](https://developer.android.com/tools/releases/platform-tools) (ADB for Option 4)
- Coverage of the first-generation tablet cutoff — [The Verge, via oso](https://oso.pages.dev/posts/older-peloton-bikes-will-soon-be-switched-off-unless-you-upgrade/) (software support ended; class access ended June 2024)

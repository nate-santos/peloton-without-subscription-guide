# Peloton Without a Subscription — Guide for the RB1VO-2 Tablet

How to keep using a Peloton bike after you drop — or never start — the
Peloton All-Access Membership: what still works stock, how to put other apps on
the big screen with **OpenPelo** (no root), and how to ride in Zwift, MyWhoosh,
TrainerRoad and friends using a data bridge or a **SmartSpin2k**.

**Scope of this guide:** Peloton tablets with model **PLTN-RB1VO-2**
(FCC ID 2AA3N-RB1VO2). No specific bike, owner, serial number, or purchase
details are covered — this is a general guide for that tablet model only.

Most of this also applies to other original-Bike tablets (RB1V1, RB1VQ,
RB1VO, RB1VQ-2). Exact menu names and what still works vary by firmware —
Peloton changes this over time, so re-check after any Peloton software update.

> **Not affiliated with Peloton.** Nothing here gets you Peloton classes for
> free — Peloton's class library stays behind its membership. This guide is
> about using *hardware you own* with your own apps, other services you
> subscribe to (or free ones), and open-source tools. Sideloading is not
> supported by Peloton: it can void warranty/support, a Peloton update can
> remove or break sideloaded apps, and a bad install can leave you doing a
> factory reset. Work in the order below, starting with the no-mod options.

---

## Quick links

**Start here**
- [OpenPelo — GitHub](https://github.com/doudar/Openpelo) · [OpenPelo releases (download)](https://github.com/doudar/Openpelo/releases) · [OpenPelo + Grupetto + SmartSpin2k walkthrough (video)](https://www.youtube.com/watch?v=Y2TonDgQtys)
- [Peloton Support — what you can access without a membership](https://support.onepeloton.com/s/article/Peloton-Membership-What-Content-Can-I-Access-Without-A-Peloton-Membership)
- [Peloton Support — Bike / Bike+ dimensions & specifications](https://support.onepeloton.com/s/article/360042990472-Peloton-Bike-and-BikePlus-Dimensions-Specifications-and-Power-Requirements)
- [Android SDK Platform Tools (manual ADB fallback)](https://developer.android.com/tools/releases/platform-tools)

**Apps to install on the tablet**
- [Grupetto — metrics overlay & FTMS broadcast](https://github.com/selalipop/grupetto)
- [Aurora Store — on F-Droid](https://f-droid.org/en/packages/com.aurora.store/)
- [Lawnchair launcher — GitHub releases](https://github.com/LawnchairLauncher/lawnchair/releases)
- [Moonlight game streaming](https://moonlight-stream.org/) · [Moonlight Android — GitHub](https://github.com/moonlight-stream/moonlight-android)

**Getting your data out / automatic resistance**
- [SmartSpin2k — GitHub](https://github.com/doudar/SmartSpin2k) · [SmartSpin2k docs](https://docs.smartspin2k.com/) · [Compatibility](https://docs.smartspin2k.com/compatibility.html) · [Peloton setup wiki](https://github-wiki-see.page/m/doudar/SmartSpin2k/wiki/Peloton-Setup) · [Prebuilt kits](https://github-wiki-see.page/m/doudar/SmartSpin2k/wiki/Prebuilt-Kits)
- [DFC (Data Fitness Connector) — Intelligenate official page](http://intelligenate.com.s3-website-us-east-1.amazonaws.com/dfc.html) · [DC Rainmaker hands-on review](https://www.dcrainmaker.com/2021/04/peloton-broadcaster-launches.html)
- [QZ / qdomyos-zwift — GitHub](https://github.com/cagnulein/qdomyos-zwift) · [QZ guide](https://robertoviola.cloud/qdomyos-zwift-guide/) · [QZ on Google Play](https://play.google.com/store/apps/details?id=org.cagnulen.qdomyoszwift&hl=en&gl=US&pli=1) · [QZ Fitness](https://www.qzfitness.com/)
- [PeloMon — DIY serial-protocol bridge](https://github.com/ihaque/pelomon)

**Training apps to ride in**
- [MyWhoosh — free](https://mywhoosh.com/)
- [TrainerRoad](https://www.trainerroad.com/blog/trainerroad-rouvy-how-to-do-trainerroad-workouts-in-rouvy/) · [TrainerRoad equipment checker](https://www.trainerroad.com/equipment-checker)

**About this tablet model**
- [Pelo Buddy — RB1VO2 / PLTN-RB1VO-2 FCC filing](https://www.pelobuddy.com/new-tablet-2aa3n-rb1vo2-pltn-rb1vo-2/)
- [FCC user manual — 2AA3N-RB1VO2 (PDF)](https://fccid.co/api/files/s1/2aa3n_rb1vo2_usersmanual_5337960_35c251106c.pdf)

---

## TL;DR — what people actually end up doing

| Your goal | Best option | Mods? | Extra cost |
|---|---|---|---|
| Just ride, see my metrics, spend $0 | **Stock "Just Ride"** + phone/tablet for music/video | None | $0 |
| Watch Netflix / YouTube / browse **on the Peloton screen** while riding | **OpenPelo** + Lawnchair launcher + Aurora Store / browser | Software only, no root | $0 |
| Ride in **Zwift / MyWhoosh / Rouvy** with real power & cadence | **DFC data broadcaster** (data out only), or power-meter pedals | Hardware dongle, no software mod | Hardware |
| Zwift etc. **with automatic resistance** (hills, ERG workouts) | **SmartSpin2k** (open source, motor turns your knob) | Hardware + companion app | Hardware |
| Structured training, no extra monthly fee | SmartSpin2k / DFC + **MyWhoosh** (free) instead of Zwift | As above | $0 app |
| Overlay live metrics / broadcast FTMS from the tablet itself | **Grupetto**, installed via OpenPelo | Software only | $0 |

The community consensus starting point, and the one OpenPelo itself ships as
its **Recommended** bundle, is:

**OpenPelo → Lawnchair (launcher) + Aurora Store (app store) + Grupetto
(metrics/FTMS) + Material Files + Moonlight**, plus a browser and your
streaming apps of choice. Add a DFC if you only need data out, or a
SmartSpin2k if you want apps to drive your resistance.

---

## 1. What still works with no membership at all (stock, no mods)

Per Peloton's own support page, without an All-Access Membership you keep:

- **Just Ride** — ride with no instruction. The screen still shows live
  cadence, resistance, output (watts), speed, distance and calories.
  Metrics display during the ride but are **not saved to your profile**.
- **3 pre-recorded classes** — pre-selected by Peloton, subject to change,
  and they don't record/save your metrics either.
- Full mechanical use of the bike — manual magnetic resistance, belt drive.

You lose: the live and on-demand class library, Leaderboard, scenic rides,
saved workout history/progress, and Peloton Entertainment *as Peloton
offers it* (the built-in Netflix/Disney+/Kindle/YouTube TV hub is an
All-Access feature — those services also each need their own subscription).

**Zero-mod entertainment trick:** Start Just Ride so the Peloton screen is
your metrics display, and play everything else — a free YouTube spin class,
music, a podcast, TV — on a phone or tablet in a handlebar mount. Ugly,
bulletproof, nothing to break on the next Peloton update. A lot of owners
land here permanently.

Sources: Peloton support, “What Content Can I Access Without a Membership?” —
<https://support.onepeloton.com/s/article/Peloton-Membership-What-Content-Can-I-Access-Without-A-Peloton-Membership>

---

## 2. The key fact about the original Bike: it does not broadcast your data

Neither the original Bike nor the Bike+ broadcasts power or cadence over
Bluetooth on its own. On the original Bike the sensor data travels **by wire**
— a 3.5 mm sensor cable from the bike up to the tablet.

That one fact decides everything below:

- An app on your phone (Zwift, QZ, TrainerRoad…) **cannot** just “find” an
  original Peloton the way it finds a Bluetooth smart bike. Something has to
  get the wired data out: a DFC, a SmartSpin2k with its Y-splitter cable,
  power-meter pedals, or software running on the tablet itself (Grupetto).
- **QZ (qdomyos-zwift) on its own does not solve this for an original
  Peloton.** QZ is an excellent, widely-loved bridge app — for Bluetooth
  bikes (Echelon, Domyos, Schwinn IC4/Bowflex C6, Keiser, etc.) and for
  adding Peloton *auto-follow* to those bikes while you watch a Peloton class
  on a second device. On an original Peloton it becomes useful **paired with
  SmartSpin2k** (see §5), not as a standalone connector. Guides that imply
  “just install QZ and your Peloton talks to Zwift” are skipping the bridge.

---

## 3. Best software option: OpenPelo (no root)

**OpenPelo** — <https://github.com/doudar/Openpelo> — MIT-licensed,
cross-platform (Windows / macOS / Linux), actively maintained (created
Dec 2024, hundreds of commits and regular releases as of Oct 2026).
It's a desktop tool that talks to the tablet over Android Debug Bridge
(ADB) — the same Developer Options / USB-debugging path Android itself
provides. **No root, no bootloader exploit, no custom ROM.** Protected
Android system areas stay inaccessible, which is exactly why it's the
low-risk option: you're installing normal apps, not reflashing the tablet.

What it does:

- Install apps from a **device-tailored catalog** (it checks each APK's
  minimum Android version, CPU architecture and signing before offering it)
  or install **your own local APK**.
- **Installed App Manager** — launch, force-stop, clear data, uninstall.
- **File Manager** — browse/transfer files both ways.
- **Launcher tools** — set a default launcher (e.g. Lawnchair), rotate the
  screen, apply developer settings, enable wireless ADB.
- **Remote screen** — live view, screenshots and MP4 recording via scrcpy.
- **Peloton-specific tools** — including Enable/Disable the built-in
  Netflix app (with a Restore Defaults action), and an advanced
  “Uninstall Peloton Apps” tool (don't use that one casually — removing
  built-ins can break normal Peloton features).
- It bundles its connection tools — **no separate ADB install needed.**

### 3.1 Before you start

- [ ] A computer (Windows, Mac or Linux) on the same Wi-Fi as the bike
- [ ] A **USB data cable** that fits the tablet's USB port (data-capable,
      not charge-only — a very common failure cause)
- [ ] The bike plugged in and the tablet awake
- [ ] Note your current Peloton software version:
      *Settings → Device Settings → About Tablet*
- [ ] Decide in advance that you're OK with a factory reset as the worst
      case recovery. Back up nothing you can't lose — there's little user
      data on these tablets beyond logins.

### 3.2 Install OpenPelo on your computer

1. Go to <https://github.com/doudar/Openpelo/releases> and download the
   build for your OS:
   - Windows: `OpenPelo_Setup_Windows.exe`
   - macOS: `OpenPelo-macOS.dmg`
   - Linux: `OpenPelo-Linux.tar.gz` (on Ubuntu 24.04 install `libmpv2` and
     `libepoxy0` first; package names vary by distro)
2. Install/open it. It sets up its connection tools automatically on
   first launch.

### 3.3 Connect the tablet (USB first, Wi-Fi after)

1. On the tablet, enable **Developer Options**:
   *Settings → Device Settings → About Tablet*, then tap **Build Number**
   repeatedly (7 taps is the standard Android gesture) until Developer
   Options unlocks. **Menu locations depend on the device and its
   firmware** — use the **Developer Mode Guide** on OpenPelo's home screen;
   it's a walkthrough matched to this flow.
2. In Developer Options, enable **USB debugging**.
3. Connect the computer to the tablet with the USB data cable.
4. On the tablet, accept the **Allow USB debugging** prompt.
5. In OpenPelo, click **Refresh Devices** if needed and select the tablet
   under **Target Device**. The status banner and **ADB Messages** panel
   show connection progress.
6. *Optional but recommended:* set up **Wi-Fi ADB through USB** (OpenPelo's
   wireless connection form → **Pair using USB**). Once “Wireless ADB
   Enabled” appears you can unplug and keep managing the tablet over your
   home network. OpenPelo remembers supported devices and reconnects
   automatically; if it stops after a restart/network change, redo
   Connect via WiFi / Pair using USB. Only do this on a trusted home
   network.

### 3.4 Install the recommended bundle

1. With the device selected, OpenPelo populates **Compatible
   applications** for your exact tablet (Android version/API, CPU, RAM
   and storage are shown on the device tile).
2. In the **Recommended** category (opens by default), use
   **Install all recommended**, or check apps individually and click
   **Install selected**. As of Oct 2026 the recommended set includes,
   when compatible:
   - **Lawnchair** — a normal Android launcher/home screen, so your apps
     are actually reachable. Set it as default afterwards (see 3.5).
     *Tip from the OpenPelo community: enable Lawnchair's app-drawer /
     gesture access so you can get to your apps without leaving a ride
     flow.*
   - **Aurora Store** — an open-source client for free apps from Google
     Play's catalog, usable without Google Play Services on the tablet.
     Project page: <https://f-droid.org/en/packages/com.aurora.store/>
   - **Grupetto** — live ride-metrics overlay and Bluetooth FTMS bridging
     from the tablet itself (see §4).
   - **SmartSpin2k companion app** — needed if you add the hardware (§5).
   - **Material Files** — a clean file manager.
   - **Moonlight** — stream a game/desktop from a PC on your network
     (also a neat way to put a big PC screen's content on the tablet).
   - **Google Text-to-Speech** (archived build) if you want spoken cues.
3. Other catalog categories: **Fitness, Launchers, App stores, Browsers,
   Files, Media & streaming, Accessibility.** Good picks for this bike:
   - A **browser** (for YouTube and other web video — see the Play
     Services warning below)
   - Your **streaming apps** (Netflix etc. — each still needs its own
     subscription and login)
   - **Zwift / MyWhoosh / Rouvy / TrainerRoad APKs** if you want the app
     *on the Peloton screen itself* — they still need a data source from
     §4/§5 to know how hard you're riding; on their own they show the
     virtual world but get no power/cadence from this bike
   - A **media player** (e.g. VLC/Kodi/Plex client) for local/network video
4. To install an APK OpenPelo doesn't list: **Install local APK** and pick
   the `.apk` on your computer. Get APKs only from the vendor, Aurora
   Store, F-Droid, or APKMirror — never random APK download sites.
   OpenPelo re-checks the APK before installing and preserves the
   original signing identity (it does not repackage/re-sign apps).

### 3.5 Make it usable day-to-day

1. **Set the launcher:** *OpenPelo → Tools → Set Default Launcher* →
   **Set Default** next to Lawnchair. (Install a launcher first.)
2. **Open your apps on the bike:** either from the Lawnchair home screen,
   or the stock route — *Settings → Device Settings → Apps → [app] → Open*.
3. **Rotation:** *Tools → Rotate Screen* if an app opens sideways;
   some apps have their own rotation setting.
4. **Built-in Netflix:** *Tools → Enable/Disable Built-in Netflix* →
   **Enable Netflix**. If enabling it affects other Peloton features,
   go back and choose **Restore Defaults** in the same tool.
5. **After any Peloton software update:** expect sideloaded apps or the
   default launcher to be reset/removed. Reconnect OpenPelo and reinstall
   — keep the computer-side setup; it's a 5-minute job once you've done
   it once. Export your installed-app list first via
   *Tools → Installed App Manager → Export visible list*.

### 3.6 Hard warnings for this tablet

- **No Google Play Services.** The Peloton tablet doesn't ship with them,
  so apps that *require* Play Services (the native YouTube / YouTube TV
  apps are the classic example) typically won't run properly even if the
  APK installs. Use the browser version instead.
- **Signature Mismatch** on install means a different-signed copy is
  already installed. OpenPelo's *Uninstall & Reinstall* path deletes that
  app **and its local data** — read the prompt, don't click through.
- **Don't uninstall Peloton's built-in apps** unless you know exactly
  which package you're removing and accept that Just Ride / normal
  Peloton behaviour may break.
- **This is not root.** The old Bike+ bootloader/Verified Boot root story
  does not apply here, current firmware has no verified public root
  exploit for this tablet, and you don't need one for anything in
  this guide.

Manual fallback (no OpenPelo): install Google's official **SDK Platform
Tools** (<https://developer.android.com/tools/releases/platform-tools>),
enable Developer Options/USB debugging as above, then
`adb devices`, `adb install <file.apk>`, `adb uninstall <package>`.
Same mechanism, no guardrails or compatibility catalog — OpenPelo is
the better tool for most people.

---

## 4. Software-only data bridging: Grupetto (+ PeloMon, for tinkerers)

**Grupetto** — <https://github.com/selalipop/grupetto> — an Android app
that runs *on the Peloton tablet* (install it via OpenPelo), reads the
bike's metrics, shows them as an overlay, and can broadcast them as a
standard Bluetooth **FTMS** fitness-machine device, as well as pair
external sensors (heart rate, power meters).

- Best use on an original Bike: metrics overlay + getting your ride data
  out to another app/device without extra dongles.
- Limit that catches people out: **direct/automatic resistance control
  in Grupetto is a Bike+ feature.** A regular original Bike has a manual
  knob no software can turn — for automatic resistance on this bike you
  need the SmartSpin2k motor (§5).

**PeloMon** — <https://github.com/ihaque/pelomon> — the DIY ancestor of
this whole space: it decodes the Peloton's wired sensor protocol and
re-broadcasts it over Bluetooth LE to a watch, Zwift, Wahoo, etc.
Fascinating, well documented, and largely superseded for most people by
the DFC (§5) unless you specifically want a build-it-yourself project.
**Gymnasticon** is a similar Raspberry-Pi-based bridge that lists the
Peloton Bike (with an extra cable) among its tested bikes.

---

## 5. Hardware bridges: DFC, power pedals, SmartSpin2k

### 5.1 DFC (Data Fitness Connector) — simplest data-out, the “just works” pick

A small box from Intelligenate that sits inline on the sensor cable:
bike sensor cable → DFC → tablet, with a USB cable to the tablet's USB
port just for power. No software to install, nothing changed on the bike
— “effectively just like plugging headphones in.”

It decodes the power/cadence(/speed) the bike sends the tablet and
re-broadcasts it over Bluetooth Smart using the **standard power-meter
profile**, so Zwift, TrainerRoad, Rouvy, MyWhoosh, or a Garmin/Polar/
Wahoo/Suunto watch sees your Peloton as a normal power meter.

- Install: plug it in (above), start a ride/Just Ride, pair it as a
  Bluetooth power meter in your app/watch. Done.
- DC Rainmaker, after ~4 months with one: “one of the only flawlessly
  perfect devices I've ever tested. It just works.” Reported accuracy
  matched the Peloton's own numbers within ~1–3 W in his testing.
- Limits: **data out only — no resistance control.** You still turn the
  knob yourself in Zwift. Peloton's displayed numbers continue as normal.
- **Availability warning (checked Oct 2026):** Intelligenate's own DFC
  page says **“Production is currently paused in 2026 as we evaluate
  options for hardware updates.”** Treat new units as unavailable until
  that page changes — look for a used unit, or use power-meter pedals
  (§5.2) / SmartSpin2k (§5.3) instead. Don't pay a scalper premium for
  a paused product.

Official page: <http://intelligenate.com.s3-website-us-east-1.amazonaws.com/dfc.html>
Write-up: <https://www.dcrainmaker.com/2021/04/peloton-broadcaster-launches.html>

### 5.2 Power-meter pedals — most accurate, most portable, priciest

Favero Assioma, Garmin Rally, etc. Swap them onto the Peloton's cranks
(standard pedal threads — keep the Delta-cleat change in mind, most power
pedals use Look Kéo or SPD-SL/SPD cleats), pair them to Zwift/MyWhoosh/
your watch as power + cadence, and ride with **manual resistance**.

Pros: true measured power, works on any bike, zero Peloton mods.
Cons: cost, cleat swap, still no auto resistance. This is the route
Zwift-forum regulars have recommended for years as the no-fuss option
if you already own (or want) power pedals for an outdoor bike too.

### 5.3 SmartSpin2k — the full smart-trainer conversion (open source)

**SmartSpin2k** — <https://github.com/doudar/SmartSpin2k> ·
docs: <https://docs.smartspin2k.com> — an open-source device that mounts
on the bike and **physically turns the resistance knob for you** with a
motor, while passing power/cadence/heart rate through as one Bluetooth
smart trainer. Zwift, Rouvy, TrainerRoad, MyWhoosh and other FTMS apps
then behave exactly as with a smart trainer: climbs get harder, descents
ease off, and **ERG mode** holds a target wattage in structured workouts.
It also adds virtual shifting via its shifter. Available DIY (ESP32-based,
build guides + CAD on GitHub) or assembled from the project store.

Peloton-specific setup (original Bike):

1. Mount the unit and arm on the bike per the build/install guide;
   zip-tie the **breakout cable** within reach of the power and shifter
   cables.
2. Peloton only: connect a **2× female → 1× male 3.5 mm Y-splitter** —
   one female to the SmartSpin2k breakout cable, the other female to
   the bike's sensor cable, and (in Tablet Mode) the male end into the
   back of the tablet.
3. Pick a mode with the switch on the side of the device:
   - **Tablet Mode (switch up, toward the tablet)** — most people start
     here. The tablet still controls comms with the bike; SmartSpin2k
     listens passively. **You must be in a workout or a free/Just Ride
     in the Peloton app** for data to flow. Then connect Zwift/other
     apps to SmartSpin2k.
   - **Headless Mode (switch down, toward the bike)** — SmartSpin2k
     controls comms; no Peloton free ride needed. This lets you run your
     own apps directly on the tablet, or even ride with the tablet off —
     also the mode to use if a tablet is damaged. Caveat from the
     project wiki: **not all Peloton tablets will power on with the data
     cable disconnected**; a loopback made from a headphone splitter
     (one male end looped back into the female end, remaining male end
     into the tablet) can trick the bike into thinking the sensor cable
     is connected.
   - To go back to normal Peloton service: reconnect the Y-splitter to
     the tablet and flip the switch up.
4. Run **calibration** from the companion app: it walks the knob to both
   end stops (expect a minute or two and some soft “tapping” at the
   stops) and saves the range. For best results on Peloton, the docs
   advise setting the power meter in SmartSpin2k's Bluetooth scanner
   settings to **“none.”**
5. Pair SmartSpin2k in your training app as a **controllable smart
   trainer** (power + cadence + resistance), and ride.

**Adding QZ here (the one place QZ + original Peloton makes sense):**
through Qdomyos-Zwift it's possible to get **automatic resistance for
instructor-guided Peloton rides** on this setup — Tablet Mode plus a
secondary Android/iOS device running QZ. That integration is supported
by the QZ community, not by SmartSpin2k directly; ask there for current
configuration steps. QZ: <https://github.com/cagnulein/qdomyos-zwift>

---

## 6. Which training apps to ride in (once you have data)

You need a data source from §4/§5 first (DFC, pedals, SmartSpin2k, or
Grupetto). Prices below were the listed US prices found when this guide
was researched (Oct 2026) — verify before you buy, they change often.

| App | Price | Best for |
|---|---|---|
| **MyWhoosh** | **Free** | Zwift-style virtual worlds, workouts, racing — the budget favourite, and genuinely free (no premium tier at research time) |
| **Zwift** | ~$19.99/mo or $199.99/yr | Biggest community, races, group rides, most polished ecosystem |
| **TrainerRoad** | Paid | Adaptive structured training plans, no virtual world |
| **Rouvy** | Paid | Real-world route video with augmented-reality overlay |
| **Kinomap / FulGaz** | Paid | Real-route video riding |
| **YouTube (browser)** | Free | Free spin classes / scenic rides — follow along visually in Just Ride, no data integration needed |

“Free” reality check: with **no bridge at all** you can still follow any
video class on a second screen while Just Ride shows your real Peloton
metrics on the bike's screen — you just don't get auto-logged power
files, Strava uploads, ERG, or avatars that move at your real speed.
For Strava/Garmin logging with no bridge, record the ride on a watch as
an indoor ride (heart rate only), or use DFC/pedals.

---

## 7. Decision guide

- **“I just want to ride and watch stuff.”**
  Just Ride + a phone/tablet. If you want it *on the big screen*, do
  OpenPelo (§3) with Lawnchair + browser + your streaming apps.
- **“I want real training, as cheap as possible.”**
  OpenPelo for the screen + DFC for data + MyWhoosh (free). Manual knob,
  real power files, $0/month.
- **“I want the full Zwift/smart-trainer experience on this bike.”**
  SmartSpin2k (§5.3). It's the only option here that gives an original
  Bike automatic resistance / ERG, because someone has to physically
  turn that knob.
- **“I train seriously and also ride outdoors.”**
  Power-meter pedals — measure once, use everywhere — plus any app
  in §6, manual resistance on the Peloton.
- **“I don't want to touch the tablet's software at all.”**
  DFC or pedals + a separate device for the app. Nothing on the bike
  changes, Peloton updates can't break your setup.

### Not recommended / know before you go looking

- **Root exploits & custom ROMs** — a 2021 Bike+ bootloader flaw got
  headlines, was patched, and isn't a path for this tablet. High brick
  risk, no benefit over OpenPelo for installing apps.
- **SHIFT Smart Trainer** — a 2020–21 Kickstarter knob-turning device
  for Zwift. Interesting idea, but it's a crowdfunding-era product;
  verify a unit actually exists, is supported, and is returnable before
  spending anything — SmartSpin2k is the actively-maintained open
  equivalent today.
- **Random “Peloton unlock” APKs / paid unlock services** — if a site
  promises free Peloton classes or sells a mystery unlock, walk away.
  Everything legitimate in this space is either free/open-source
  (OpenPelo, Grupetto, SmartSpin2k, QZ) or a named hardware product
  (DFC, power pedals).

---

## 8. Troubleshooting

| Symptom | Check |
|---|---|
| OpenPelo shows no device | USB debugging on? **Data** cable (not charge-only)? Accepted the on-tablet authorization prompt? Click **Refresh Devices** and select the right **Target Device** |
| Wireless pairing fails | Re-read the current 6-digit code **and pairing port** from the tablet's pairing dialog; the *connection* port is a different number, from the main Wireless Debugging screen |
| Wi-Fi scan finds nothing | Same network for computer + tablet, wireless debugging on, or enter IP/ports manually. On macOS, allow OpenPelo in *System Settings → Privacy & Security → Local Network* |
| An APK won't install | Read **ADB Messages** for the reason; check Android version/architecture compatibility; on **Signature Mismatch**, understand the reinstall prompt deletes that app's data before you confirm |
| App installs but crashes / sign-in fails | It probably requires Google Play Services, which this tablet lacks. Use its web version in a browser instead |
| Apps/launcher vanished | A Peloton update reset them. Reconnect OpenPelo and reinstall (export your app list beforehand next time) |
| SmartSpin2k gives no data in Tablet Mode | You must be inside a Peloton workout / Just Ride — no ride running, no data flowing |
| Tablet won't power on in Headless Mode | Known quirk on some tablets with the data cable disconnected — use the loopback splitter described in §5.3 |
| Zwift sees power but resistance never changes | You have a data-only bridge (DFC/pedals). Resistance control requires SmartSpin2k — or your own hand on the knob |
| QZ doesn't find the bike | Expected on a stock original Peloton — it broadcasts nothing. QZ needs a Bluetooth bike or a SmartSpin2k in the chain (§2, §5.3) |

Recovery, worst case: a factory reset of the tablet
(*Settings → Device Settings → … → Factory Reset* — menu wording varies
by firmware) returns it to stock Peloton behaviour. You lose sideloaded
apps and logins, not the bike.

---

## 9. Sources

### Peloton (official)

- Peloton Support — “What Content Can I Access Without a Membership?” —
  <https://support.onepeloton.com/s/article/Peloton-Membership-What-Content-Can-I-Access-Without-A-Peloton-Membership>
- Peloton Support — Bike and Bike+ dimensions & specifications —
  <https://support.onepeloton.com/s/article/360042990472-Peloton-Bike-and-BikePlus-Dimensions-Specifications-and-Power-Requirements>

### Tablet model identification

- Pelo Buddy — “Updated Original Peloton Bike Monitor Filed with FCC
  (2AA3N-RB1VO2 / PLTN-RB1VO-2)” —
  <https://www.pelobuddy.com/new-tablet-2aa3n-rb1vo2-pltn-rb1vo-2/>
- FCC filings — user manual for FCC ID 2AA3N-RB1VO2 (PDF) —
  <https://fccid.co/api/files/s1/2aa3n_rb1vo2_usersmanual_5337960_35c251106c.pdf>

### Software — installing apps without root

- OpenPelo (README, install & tools documentation) —
  <https://github.com/doudar/Openpelo> ·
  releases: <https://github.com/doudar/Openpelo/releases>
- OpenPelo / Grupetto / SmartSpin2k walkthrough (video) —
  <https://www.youtube.com/watch?v=Y2TonDgQtys>
- Android SDK Platform Tools (for the manual ADB fallback in §3.6) —
  <https://developer.android.com/tools/releases/platform-tools>
- Grupetto — <https://github.com/selalipop/grupetto>
- Aurora Store on F-Droid —
  <https://f-droid.org/en/packages/com.aurora.store/>
- Lawnchair launcher — GitHub releases —
  <https://github.com/LawnchairLauncher/lawnchair/releases>
- Moonlight — <https://moonlight-stream.org/> ·
  Android client: <https://github.com/moonlight-stream/moonlight-android>

### Hardware bridges

- SmartSpin2k — <https://github.com/doudar/SmartSpin2k> ·
  docs: <https://docs.smartspin2k.com/> ·
  about: <https://docs.smartspin2k.com/about.html> ·
  compatibility: <https://docs.smartspin2k.com/compatibility.html> ·
  Peloton setup wiki:
  <https://github-wiki-see.page/m/doudar/SmartSpin2k/wiki/Peloton-Setup> ·
  prebuilt kits:
  <https://github-wiki-see.page/m/doudar/SmartSpin2k/wiki/Prebuilt-Kits>
- DFC — Intelligenate official page (incl. the 2026 production-paused
  notice quoted in §5.1) —
  <http://intelligenate.com.s3-website-us-east-1.amazonaws.com/dfc.html>
- DC Rainmaker — “Peloton Power Data Broadcaster (DFC) Launches:
  Hands-on Details” — <https://www.dcrainmaker.com/2021/04/peloton-broadcaster-launches.html>
- PeloMon — <https://github.com/ihaque/pelomon>
- Hackster — Gymnasticon / Raspberry Pi bridge background —
  <https://www.hackster.io/news/some-reverse-engineering-and-a-raspberry-pi-zero-w-breathe-new-life-into-bricked-flywheel-bikes-b6bf82591295>

### Bridges / companion apps

- QZ (qdomyos-zwift) — <https://github.com/cagnulein/qdomyos-zwift> ·
  guide: <https://robertoviola.cloud/qdomyos-zwift-guide/> ·
  Google Play: <https://play.google.com/store/apps/details?id=org.cagnulen.qdomyoszwift&hl=en&gl=US&pli=1> ·
  <https://www.qzfitness.com/>
- TrainerRoad forum — Grupetto on Bike+ / smart-trainer setup discussion —
  <https://www.trainerroad.com/forum/t/configure-peloton-bike-to-act-as-smart-trainer-grupetto-app/115238>

### Training apps

- MyWhoosh (free) — <https://mywhoosh.com/>
- TrainerRoad — <https://www.trainerroad.com/blog/trainerroad-rouvy-how-to-do-trainerroad-workouts-in-rouvy/> ·
  equipment checker: <https://www.trainerroad.com/equipment-checker>

*Guide created October 21, 2026. Prices, app catalogs, firmware behaviour
and project availability change — verify the linked project pages before
buying hardware or following a step that doesn't match your screen.*

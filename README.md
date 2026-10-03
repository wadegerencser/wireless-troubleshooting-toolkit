# Cisco Wireless Troubleshooting Toolkit

> A Cisco-platform-specific troubleshooting field guide for 802.11 wireless engineers working ISE, Catalyst 9800 (IOS-XE), and Catalyst Center — built from Cisco documentation and real case findings, not RF/802.11 theory.
>
> **Live app:** https://wadegerencser.github.io/wireless-troubleshooting-toolkit/

## Business Problem

New and intermediate 802.11 wireless engineers working Cisco's specific platform (ISE + Catalyst 9800 + Catalyst Center) repeatedly hit the same categories of mistakes — misdiagnosing AP-join failures as AAA/RADIUS problems, getting the CWA redirect-ACL permit/deny logic backwards (especially when carrying AireOS habits onto IOS-XE), misunderstanding ISE PSN sizing/placement, and conflating HA SSO with N+1 — without one place that ties these specific gotchas back to Cisco's own documentation.

## Why

These gaps keep surfacing across real engagements and get re-derived from scratch each time rather than captured once. The goal is a durable, growing reference other Cisco wireless engineers can use instead of relearning the same lessons independently.

## How

A documentation set built from Cisco's own ISE/9800/Catalyst Center configuration guides, TAC troubleshooting docs, and Cisco Press/CVD material, plus findings pulled from real case investigations (with customer-identifying specifics stripped out). Every claim is tagged documented / inference / unconfirmed so the guide doesn't overstate what Cisco has actually published versus what's informed field reasoning.

## Not This

This is explicitly **not** a replacement for CWNP's vendor-neutral RF/802.11 theory material (CWNA/CWDP/CWNE) — it stays scoped to Cisco-platform implementation detail, not RF fundamentals, which remain CWNP's domain. It is also not a customer case-tracking tool and contains no customer-identifying data — case-specific investigations live in their own separate, private repos.

**Known internal overlap:** an internal duplication check against indexed Cisco repositories surfaced related efforts covering parts of this same ground (wireless troubleshooting methodology, ISE deployment/scaling guidance, and ISE/802.1X NAC configuration references). The repo owner was shown these internally and chose to proceed anyway rather than pause or merge. Details of those internal repos and their owners are intentionally not published here, since this repo is public and those are other engineers' internal projects — check internally at Cisco before assuming this guide is the only or definitive resource on this topic, and this repo does not claim precedence over any internal work covering similar ground.

## Impact

Unquantified. Intended to reduce the time new/intermediate engineers spend re-deriving Cisco-specific troubleshooting knowledge that is already documented but scattered across many separate guides.

## Details

| Facet | Value |
|---|---|
| **Status** | `experimental` |
| **Owner** | mgerencs@cisco.com |
| **Related** | complements a separate private case-investigation repo · known overlap with other internal Cisco repos (not named here; see "Known internal overlap" above) |

## Session Notes — 2026-10-03

A running log of what got built and decided in this working session, so a future
session can pick up context fast without re-deriving it. Append to this section
rather than replacing it.

### What shipped
- Renamed app to **Cisco Wireless Troubleshooting Toolkit**; retitled nav/hero to match.
- Reorganized "Troubleshoot by symptom" from one flat 16-item list into **8 labeled
  categories** (35 entry points): AP & Controller Infrastructure, Client Authentication
  & AAA, Roaming (802.11r/802.11k/802.11v), Mobility & FlexConnect, Identity Services
  Engine, Application & Performance, Platform-Specific — Cloud & Outdoor, and Legacy —
  AireOS & Mobility Express (pinned last, since it's deprecated relative to 9800/IOS-XE).
- New **Mobility & FlexConnect** topic (mobility groups, anchor/foreign, CAPWAP mobility
  tunnel, FlexConnect standalone mode/site tags) — scoped to *current* IOS-XE/9800 docs.
  Legacy AireOS mobility-group terminology and EoIP stayed exclusively in the AireOS
  section, per explicit instruction not to default to the old AireOS mental model.
- Split old "802.11r FT roaming + CWA" topic: FT/802.11k/802.11v roaming mechanics now
  stand alone (`id="ft-roaming"`); all CWA content merged into **Identity Services
  Engine (ISE)** (`id="ise-sizing"`), since CWA is implemented through ISE.
- Added `ap-license`, `ap-crashloop`, `ap-mass-disconnect` topics.
- Public hosting set up: merged PRs on Cisco GHE (`mgerencs/cisco-wlan-ise-fieldguide`),
  mirrored to `github.com/wadegerencser/wireless-troubleshooting-toolkit`, GitHub Pages
  enabled → **this is the live URL at the top of this file**. Added as a 5th card on
  the wirelesswithwade.com Wireless Toolkits page (Page ID 590), matching the existing
  card template exactly (URWB-link-planner pattern: "Run Now" primary CTA to Pages URL).
- Removed the "topics covered / documented findings / flagged inference" stat row from
  the overview — manually-maintained counts that kept drifting out of sync.
- Added a **Speed Test** tool (`id="speedtest"`) — real download/upload throughput,
  idle + loaded latency (bufferbloat delta), and TCP connection detail (RTT, RTT
  variance, retransmits, packet loss), all measured live against Cloudflare's public,
  CORS-open test endpoints (`speed.cloudflare.com/__down`, `__up`, `cdn-cgi/trace`,
  and the `Server-Timing: cfL4` response header for the TCP stats) — the same
  infrastructure speed.cloudflare.com's own UI runs on. No backend of our own needed.

### Decisions worth remembering
- **No RSSI/channel/PHY-rate correlation from throughput, ever.** Explicitly ruled out
  by the project owner: "don't fluff or fabricate information whatsoever." A browser
  cannot read RSSI/SNR/channel/MCS on any OS — that's a driver/OS privacy boundary, not
  a limitation of this page. The toolkit says so directly rather than estimating radio
  state from throughput/latency numbers that are mostly determined by non-Wi-Fi factors.
- **No invented bufferbloat grading scale.** First draft of the bufferbloat feature
  cited an "A–F grade, per Cloudflare's own thresholds" that was never actually verified
  against a real Cloudflare source — caught and corrected before shipping. Current
  version reports the measured millisecond delta plus a plain-language read of it, with
  no invented letter-grade thresholds attributed to anyone.
- **No remote "paste in a device IP and pull its logs" feature.** Considered and
  rejected: a public web page that fetches from an arbitrary user-supplied IP is the
  textbook shape of an SSRF/CSRF-against-internal-devices vector (a malicious page could
  use the same mechanism against a visitor's own internal network). Cisco gear also
  doesn't expose a CORS-enabled log-read endpoint, so it wouldn't actually work even
  ignoring the security problem. **Decided instead:** add a reference section (same
  pattern as "Pulling your own connection details") with the real Cisco commands for a
  live capture (`monitor capture` / EPC) and a crash/support bundle
  (`show tech-support wireless`) — honest reference content, not a fake live feature.
- **No in-browser packet capture (tcpdump equivalent).** Not a toolkit limitation —
  no browser on any OS exposes raw socket access to a web page. Ruled out entirely,
  not worth revisiting unless the browser security model itself changes.
- **Every claim needs the documented/inference/unconfirmed badge pattern.** Applies to
  new sections too, not just the original reference topics — the Speed Test's "Sources"
  list explicitly notes where a claim came from a live `curl`-verified response header
  rather than a published Cloudflare doc page, since none was found describing that
  header's exact field semantics.

### Round 2 — same-day follow-up (audit fixes, 5 new tools, visitor counter)

This went out to the project owner's management, so the bar was "verify everything,
don't invent anything" rather than speed. Three research agents ran: a full-file
consistency audit, a Cisco-command verification pass (crash/support-bundle commands),
and a feasibility check on 5 candidate new tools. All three landed clean — no
fabrication caught in any of them, which is itself a useful signal about how the
documented/inference/unconfirmed discipline held up under scrutiny.

**Audit findings fixed:**
- Nav "count" badges and the overview's hero stats are now **computed live via JS from
  the DOM** (real `<h3>`/`<h4>` counts per topic, real `.status.good/.caution/.alert`
  badge counts page-wide) instead of hand-maintained numbers — this is the second time
  stale hardcoded counts caused a problem, so the fix this time removes the possibility
  entirely rather than just recalculating by hand again.
- Fixed ~4 confirmed panel-color / confidence-badge mismatches (panel border color
  should track confidence level — good/caution/alert — not operational severity;
  found concentrated in the Mobility & FlexConnect section plus one in Multicast).
- Added `aria-live="polite"` to the Speed Test's result grids (values were updating
  silently for screen-reader users; only the status text line was announced before).
- Confirmed via live `curl` and a full-file structural audit: zero broken internal
  links, zero stale cross-references from the earlier reorg, zero duplicate ids,
  balanced HTML throughout. The Speed Test's technical claims were independently
  re-verified against live Cloudflare response headers and found accurate.
- Source citations across the whole app are plain text, not clickable links — checked
  specifically after the project owner raised a concern about dead links reaching
  customers/employees. Confirmed nothing is a broken `<a href>` (nothing is a link at
  all yet) — flagged as a possible future improvement, not fixed in this pass.

**New: packet capture / crash-bundle reference** (in the Speed Test topic) — real,
individually-verified Cisco commands, not guessed syntax:
- `monitor capture uplink ...` (EPC) — confirmed from Cisco's own troubleshooting doc.
- `show tech-support wireless` (+ `ap`/`client`/`mobility`/`datapath` sub-keywords) —
  confirmed **verbatim from Cisco's official Catalyst 9800 Command Reference PDF**,
  not a summary. This is the real TAC-bundle-equivalent command.
- `request tech-support` (IOS-XE 17.15.1+) — confirmed documented wrapper that runs
  `show tech-support` and bundles it with the system report into one file.
- `show ap crash-file` / `bootflash:system-report-*.tar.gz` — the actual crash/coredump
  artifacts, distinct from the general tech-support bundle.
- `request platform software trace archive` was deliberately **excluded** from being
  presented as a tech-support equivalent — confirmed it only archives trace logs, a
  narrower thing, and the toolkit says so explicitly rather than conflating the two.

**New: 5 tools added** (nav group "Calculators & lookups", right after Speed Test):
1. **Channel & regulatory lookup** (`id="channel-lookup"`) — pure calculator, no network
   calls. 2.4/5/6 GHz channel↔frequency (`freq = 2407/5000/5950 + 5×channel`), UNII band
   membership, DFS requirements, 6 GHz LPI/Standard-Power/VLP + AFC. Verified the
   frequency formulas by hand against known textbook values (ch36=5180MHz, ch149=5745MHz)
   before shipping.
2. **802.11 standards matrix** (`id="standards-matrix"`) — pure static reference table,
   802.11a through 802.11be/Wi-Fi 7, ratification years, max PHY rates, key features.
3. **PoE budget calculator** (`id="poe-calc"`) — pure calculator extending the existing
   static PoE reference section; sizes against full nameplate 802.3af/at/bt class draw
   (the conservative figure), not estimated real-world draw. Cross-links to the existing
   PoE & AP power topic for field triage vs. this tool's deployment-planning use case.
4. **Egress / ASN lookup** (`id="asn-lookup"`) — **real network calls**: Cloudflare's
   `cdn-cgi/trace` (same endpoint the Speed Test uses) for public egress IP, then RIPE
   NCC's public RIPEstat API (`stat.ripe.net`, no key, CORS-open) for the real ASN/holder/
   announced prefix. Verified end-to-end in a live browser test (real AS14618/Amazon
   result returned). Framed around confirming anchor-WLC/guest-NAT/FlexConnect egress
   paths, not generic "what's my IP."
5. **MAC vendor (OUI) lookup** (`id="oui-lookup"`) — **real network call**: fetches
   Wireshark's maintained mirror of the IEEE OUI registry (`wireshark.org/download/
   automated/data/manuf` — CORS-enabled; the canonical IEEE CSV has no CORS header and
   was correctly rejected as unusable from a browser). Caches the ~3.3MB file in
   `localStorage` for 7 days, honoring that file's own "don't fetch more than weekly"
   request rather than hitting it on every lookup. Verified live: `00:0C:29` correctly
   resolved to "VMware, Inc."

Explicitly dropped during research: a client-device Information-Element/capability-
string decoder — not honestly buildable without real captured frame bytes, which is
already the job of the existing Beacon Finder portfolio tool, not a text-paste gadget.

**New: visitor counter** — footer badge (`<img>`, no JS/CORS needed) via
`api.visitorbadge.io`, a free keyless counting service. Verified live before shipping:
hit the endpoint 3 times in a row and confirmed the count actually incremented
(2 → 3 → 4), not a static image.

**Known still-open item:** the project owner asked for the hero stats row's confidence
math (confidence-rate %, flagged count) to be reconsidered/tweaked after seeing it live —
"we will tweak this item specifically" — the current version is live-computed and
functionally correct but may get a presentation pass later. Check with the owner before
assuming the current formula/layout is final.

### Repo naming note
Cisco-internal repo is still named `cisco-wlan-ise-fieldguide` (the original working
name) while the public mirror is the cleaner `wireless-troubleshooting-toolkit` —
intentionally not renamed on the Cisco-internal side to avoid link breakage; revisit
only if that becomes confusing.

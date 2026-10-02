# cisco-wlan-ise-fieldguide

> A Cisco-platform-specific troubleshooting field guide for 802.11 wireless engineers working ISE, Catalyst 9800 (IOS-XE), and Catalyst Center — built from Cisco documentation and real case findings, not RF/802.11 theory.

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

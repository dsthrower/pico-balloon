# Pico Balloon Project — Reference Links

Quick-reference URLs for the 20m WSPR pico balloon build (Traquito Jetpack).

## This document / repo

Lives in `github.com/dsthrower/pico-balloon` (branch: `master`). Raw URLs:

- references.md — https://raw.githubusercontent.com/dsthrower/pico-balloon/master/references.md
- receive_station_setup.md — https://raw.githubusercontent.com/dsthrower/pico-balloon/master/receive_station_setup.md
- README.md — https://raw.githubusercontent.com/dsthrower/pico-balloon/master/README.md

## Related documents

- **Receive station setup & runbook:** `receive_station_setup.md` — SDR/antenna hardware, signal chain, and bring-up steps for monitoring WSPR.

## Traquito (primary project site)

- **Home:** https://traquito.github.io/
- **Jetpack tracker guide:** https://traquito.github.io/tracker/
- **Jetpack technical details:** https://traquito.github.io/tracker/V1/
- **Solar power guide:** https://traquito.github.io/solar/
- **Tracker Hacker (weight reduction):** https://traquito.github.io/pro/trackerhacker/

Note: `traquito.org` redirects here; `traquito.github.io` is the canonical address.

## General pico ballooning how-to

- **Traquito** (primary — see above): https://traquito.github.io/
- **The Astro Imager — pico ballooning (John Ruthroff):** https://www.theastroimager.com/picoballoning/pico-ballooning/
- **LightAPRS — Tips & Tricks for Pico Balloons (wiki):** https://github.com/lightaprs/LightAPRS-1.0/wiki/Tips-&-Tricks-for-Pico-Balloons
- **Pico Balloons by K9YO — balloon:** https://sites.google.com/view/picoballoonsbyk9yo/balloon
- **Pico Balloons by K9YO — transmitter:** https://sites.google.com/view/picoballoonsbyk9yo/transmitter

## Products & suppliers

- **ZachTek (WSPR transmitters):** https://www.zachtek.com/
- **Scientific Balloon Solutions:** https://www.scientificballoonsolutions.com/products/
- **QRP Labs — LightAPRS-W:** https://qrp-labs.com/lightaprs-w
- **Yokohama Balloon:** https://yokohamaballoon.com/
- **QRP Labs — U4B tracker kit:** https://shop.qrp-labs.com/otherkits/u4b

## My flight — KK7OWL

- **Callsign:** KK7OWL
- **Band:** 20m
- **Reserved channels:** 290, 291, 292

Traquito tools (channel/log/map):

- **20m channel map:** https://traquito.github.io/channelmap/
- **Flight log:** https://traquito.github.io/flightlog/
- **Flight map dashboard (base):** https://traquito.github.io/search/spots/dashboard/
- **My flight map (pre-filled, channel 290):** https://traquito.github.io/search/spots/dashboard/?band=20m&channel=290&callsign=KK7OWL

  Swap `channel=290` to 291 or 292 as needed; add `&dtGte=YYYY-MM-DD&dtLte=YYYY-MM-DD` to bound a date range.

## Key specs (from build)

- **Jetpack input voltage:** 3.0V – 5.5V (buck/boost regulator); hardware reset floor at 2.6V
- **Current draw:** max 70mA @ 3.3V (higher voltage = lower current)
- **20m WSPR dipole:** 4325mm per leg (30 AWG tinned copper), tip-to-tip 8650mm; resonant 14.020 MHz, SWR 1.33 on bench

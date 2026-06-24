# WSPR Receive Station — Setup & Runbook

Receive/monitor station for 20m WSPR (used to verify the Traquito Jetpack tracker
before flight, and to watch for spots once airborne).

## Hardware

- **Computer:** 2021 MacBook Pro, M1 Pro, 16 GB RAM (macOS)
- **SDR:** RTL-SDR Blog V3 (RTL-SDR.COM Model V3), via JSAUX USB-A adapter
- **Antenna:** MegaLoop MLA-30+ active loop antenna — receive only, 0.5–30 MHz
- **Bias tee / injector:** MLA-30+ biasing tee box
  - USB 5V powered, "ANT PWR ON" LED confirms the loop is powered
  - Ports: **To Antenna** (up to the loop head) and **To Receiver** (down to SDR)

## Software

- **SDR++ v1.3.0** — receiver / tuning
- **BlackHole (2ch)** — virtual audio cable (macOS), routes SDR++ audio to the decoder
- **WSJT-X** — WSPR decode endpoint (fed by BlackHole)

## Signal chain

```
  MegaLoop MLA-30+ loop head  (in the air, receive-only active antenna)
            │  coax
            ▼
  MLA-30+ bias tee box ── USB 5V power (injects DC up the coax to the loop)
   "To Receiver" SMA
            │  coax
            ▼
  RTL-SDR Blog V3 ── USB ──► MacBook Pro
            │
            ▼
  SDR++ v1.3.0   (Direct Sampling: Q branch, tuned to 20m WSPR)
            │  audio out → BlackHole 2ch
            ▼
  BlackHole 2ch  (virtual audio cable)
            │  audio in
            ▼
  WSJT-X  (WSPR mode, 20m)  → decodes spots
```

## Key settings

### RTL-SDR Blog V3 in SDR++
- Source: **RTL-SDR**
- **Direct Sampling: Q branch** — REQUIRED for HF. The V3's tuner can't reach 14 MHz
  directly; HF comes in through the direct-sampling Q input.
- **Do NOT enable the RTL-SDR's internal bias tee.** The loop is powered by the
  external MLA-30+ box. (The internal bias tee also doesn't function in direct-
  sampling mode.)

### Tuning — 20m WSPR
- WSPR 20m dial frequency: **14.0956 MHz, USB**
- WSPR audio passband: **1400–1600 Hz** (signals land ~1500 Hz in the audio)
- Actual on-air WSPR segment: ~14.0970–14.0972 MHz

### Audio routing (SDR++ → WSJT-X)
- SDR++ audio **output** → **BlackHole 2ch**
- WSJT-X audio **input** → **BlackHole 2ch**
- WSJT-X mode: **WSPR**; band: **20m**
- Keep the computer clock tightly synced (NTP) — WSPR decoding depends on accurate
  time (transmissions start at even-minute boundaries).

## Bring-up checklist
1. Connect loop → bias tee "To Antenna"; bias tee "To Receiver" → RTL-SDR.
2. Plug bias tee USB 5V — confirm **ANT PWR ON** LED is lit.
3. Plug RTL-SDR into the MacBook.
4. Launch SDR++ → select RTL-SDR source → enable **Direct Sampling: Q branch**.
5. Set output audio device to **BlackHole 2ch**.
6. Tune to **14.0956 MHz USB**.
7. Launch WSJT-X → input = **BlackHole 2ch**, mode = **WSPR**, band = **20m**.
8. Confirm clock sync, then watch for decodes on even-minute boundaries.

## Notes / troubleshooting
- **No HF signals at all** → almost always Direct Sampling not set to Q branch.
- **No decodes but waterfall looks alive** → check audio routing (both SDR++ out and
  WSJT-X in must be BlackHole 2ch) and clock sync.
- **Loop seems dead** → check the ANT PWR ON LED; no LED = loop unpowered (USB / bias
  tee issue).
- MLA-30+ remote supply spec: 5–12V / max 40mA (here powered from the USB 5V box).

# WSPR Bench Test Procedure — KK7OWL Jetpack

End-to-end procedure to confirm a Jetpack WSPR tracker transmits, gets GPS lock,
and is heard by the WSPR network. Validated 2026-06-24 — full success.

## Scope / prototype note

**This procedure tests ONE prototype tracker on bench/temporary power — it is NOT
the solar-powered flight unit.** Power here is a battery (lithium cells), not the
flight solar array. The purpose is to prove the RF + GPS + telemetry chain works.
A final pre-flight test on the actual solar panels is still required, since solar
behavior under load (and voltage sag in weak light) is not exercised here.

## Equipment used

Tracker side:
- Jetpack tracker (RPi Pico + Add-on Module), firmware 2025-12-09
- Tuned 20m dipole: 4325 mm per leg, 30 AWG tinned copper (resonant ~14.020 MHz)
- GPS antenna: insulated wire, ~5 cm, on the left GPS pad, vertical
- Power: **3 lithium AA/AAA cells in series (~4.95 V measured)** — bench test only

Receive side (see receive_station_setup.md for full detail):
- MacBook Pro (M1 Pro)
- RTL-SDR Blog V3 + MegaLoop MLA-30+ active loop + MLA-30+ bias tee box
- SDR++ v1.3.0, BlackHole 2ch, WSJT-X (WSPR mode)

## Power — important lesson

**Do NOT power the tracker from a typical USB power bank for testing.**
Most power banks have a low-current auto-shutoff. The Jetpack draws under ~70 mA,
which is below many banks' cutoff threshold, so the bank powers itself off after a
minute or two ("battery timed out"). Use cells instead:
- 2–3 AA/AAA lithium cells in series (lands in the 3.0–5.5 V input window)
- or a small LiPo
Lithium (not alkaline) is preferred — holds voltage better, works in cold.
Observe polarity: top two Jetpack pads are power, + to +.

## Configuration mode vs flight mode

- **USB to computer** = Configuration Mode. Radio idle except manual "Send".
  Use the Tracker GUI (traquito.github.io/trackergui) to set band/channel/callsign
  and to fire single test transmissions.
- **Battery (power, no data)** = Flight Mode. Transmits on its scheduled even-minute
  slots once GPS has a 3D lock. This is the realistic test.

Config used: Band 20m, Channel 291, Callsign KK7OWL, Grid CN94 (Bend).

## GPS notes

- Tracker needs a clear view of the sky to lock — receiver does NOT.
- "ANTENNA OPEN" in the NMEA stream is normal/harmless for a passive wire GPS
  antenna. Judge lock by satellites appearing ($GPGSV count > 0) and $GPGSA
  showing 3 (3D fix), not by the ANTENNA message.
- Occasionally a transmission goes out before full lock — it reports a coarse or
  stale position. Normal; most transmissions at altitude will have good lock.

## SDR++ settings (the two that matter most)

- **Direct Sampling: Q branch** — REQUIRED. The V3 tuner can't reach 14 MHz
  directly; without this the waterfall is dead. #1 cause of "no HF".
- **Internal Bias T: OFF** — the external MLA-30+ box powers the loop, and the
  internal bias tee doesn't work in direct-sampling mode anyway.
- Mode USB, tune **14.0956 MHz** (the 20m WSPR dial frequency).
- Gain ~37–40 dB. Audio sink → BlackHole 2ch.

## WSJT-X settings

- Input device: BlackHole 2ch. Mode: WSPR. Band: 20m (dial 14.0956).
- **My Grid: CN94** — set this, or local decode distances are nonsense.
- **Clock must be NTP-synced** (macOS "Set time automatically" ON). WSPR decodes
  on even-minute boundaries; a 2 s offset = zero decodes.
- Input level on the WSJT-X meter should sit ~30–50. (The SDR++ bottom bars are a
  separate, post-AGC indicator — green-into-red there is fine; trust WSJT-X's meter.)

## Distance — counterintuitive but important

The tracker is a strong, close transmitter. Putting it next to the receive antenna
OVERLOADS the SDR front end — you get nothing, or smeared garbage, NOT a clean
decode. Symptoms of overload: a single transmission decoded multiple times at
~60 Hz-spaced frequencies, and bogus high (+30) SNR values.
- Keep tracker and receive antenna **at least 5 m apart**, ideally with a wall.
- If not decoding a nearby tracker, increase separation, don't decrease it.

## Test run — steps

1. Power tracker from the 3-cell lithium pack. Confirm LED blinking.
2. Place it outside with clear sky view, >5 m from the receive antenna.
3. Confirm GPS 3D lock (LED pattern, or check briefly on USB in the GUI).
4. Bring up the receive chain (SDR++ → BlackHole → WSJT-X) per the settings above.
5. Watch WSJT-X. Flight Mode sends on scheduled even-minute slots (NOT every cycle),
   so allow 10+ minutes. Decodes accumulate in the panel and persist.
6. A successful decode shows: KK7OWL, grid CN94, power 13, with SNR and frequency.

## Confirming success — trust the network, not just local RX

The most reliable confirmation is the **Traquito Flight Dashboard**, not your own
receiver. If the tracker is transmitting and ANY station worldwide hears it, it
appears there:
  https://traquito.github.io/search/spots/dashboard/?band=20m&channel=291&callsign=KK7OWL

This is also how real flights are tracked — via the global WSPR receiver network,
not your home station. On 2026-06-24 the dashboard confirmed KK7OWL with GPS 3D,
correct Bend position (44.0208, -121.375), ~4.95 V, and RxStationCount of 3–4
(multiple stations worldwide hearing each transmission).

## Result (2026-06-24)

PASS. Tracker transmits, GPS locks (3D), telemetry encodes correctly, position is
accurate, power is healthy on the 3-cell lithium pack, and multiple network
stations received it. RF + GPS + telemetry chain proven on the prototype.

## Still to do before flight

- Final test on the actual SOLAR power array (this test was battery only).
- Cold-soak / environmental checks as applicable.
- Strain-relief and ruggedize antenna/feedpoint joints for flight.

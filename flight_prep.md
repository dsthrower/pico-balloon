# Pico Balloon Flight Prep — KK7OWL

Single source of truth for balloon selection, the hydrogen fill chain, and the
leak-test → stretch → fill → seal procedure. 20 m WSPR pico balloon build on the
Traquito Jetpack, targeting long-duration float / circumnavigation.

---

## 1. Flight identity

| Field | Value |
|---|---|
| Callsign | KK7OWL |
| Band / mode | 20 m WSPR |
| Dial / TX freq | ~14.097 MHz |
| Reserved channels | 290 / 291 / 292 |
| Launch grid | CN94md (Bend, OR) |
| Tracker | Traquito Jetpack (solar) |

---

## 2. Balloon — what definitively flies

Selection filter: **documented circumnavigations**, not spec sheets. The flown
size is the **32" sphere** (stretches to ~100" circumference; the big "50/60 in"
labels on decorative orbs are circumference-ish marketing, not flight-grade).

**Proven, ranked:**

1. **Yokohama 32" Clear Sphere** — gold standard, clearest, most consistent.
   Records include 131-day flights and 12-lap circumnavigations.
   Sold only in 10-packs, ~$125 + ~$100 freight from Japan (~$225–250 landed).
   https://yokohamaballoon.com/products/sphere-balloon32inch-1bag10pieces
   (confirm clear / valveless at checkout)

2. **SAG Orbs 32" Clear** — US-sourced, ~$11.83, proven (3 circumnavigations on
   record). Slightly less consistent than Yokohama per community notes.
   https://balloons.online/orbs-32-clear/

3. **SAG Orbs 32" Silver** — US-sourced, ~$11.83, proven (123-day / 8-lap flight).
   Silver heats more under sun than clear (minor thermal penalty).
   https://balloons.online/orbs-32-silver/

**Avoid:** big 50"/60" Airise / Decochamp "iridescent clear" orbs — heavy
(~138 g), thick-walled, spec'd air-only. Reviewed by a ham as unsuitable for
telemetry. Not flight-grade.

**Practice vs. flight:** PartyWoo 50" 4D foil (already on hand) = fine first /
practice flight. SAG Orbs 32" Clear = the real circumnavigation envelope.

**Community sourcing hub:** picoballoon@groups.io — https://groups.io/g/picoballoon
(first post moderated). Where current "where to buy / what's working" gets answered.

---

## 3. Tracker power (Traquito Jetpack)

| Parameter | Value |
|---|---|
| Input range | 3.0 – 5.5 V (buck/boost regulated) |
| Hardware reset floor | 2.6 V |
| Max current | 70 mA @ 3.3 V (draw drops as voltage rises) |

Solar panel voltage may swing with sun angle; the regulator holds the tracker
stable across 3.0–5.5 V and powers down gracefully at night.

---

## 4. Hydrogen fill chain

```
[H2 cylinder]
   │
[CGA 350 regulator]                         ← from Norco (confirm outlet thread!)
   │  9/16-18 LH male outlet (standard fuel-gas "B"); metal-to-metal seat, NO tape
[Dixon 1580904C  9/16-18 LH(F) × 1/4 NPT male]   MSC #48763429
   │  1/4 NPT  (yellow gas PTFE tape)
[Litorange needle valve  1/4 NPT female ← male]  (female faces Dixon; fine flow)
   │  1/4 NPT  (yellow gas PTFE tape)
[SUNGATOR barb  1/4 NPT female × 1/4" barb]  + hose clamp
   │
[1/4" ID vinyl tubing]
   │  (optional 2nd SUNGATOR barb + clamp)
[angle-cut straw → balloon valve]
```

**Parts + links:**

| Part | Spec | Link / ID |
|---|---|---|
| Regulator | CGA 350, hydrogen | from Norco (incoming) |
| Adapter | 9/16-18 LH × 1/4 NPT, brass | MSC #48763429 (Dixon 1580904C) |
| Needle valve | 1/4 NPT M × F, brass | Litorange (Amazon) |
| Barb + clamps | 1/4 NPT F × 1/4" barb, 2-pk | SUNGATOR (Amazon) |
| Tubing | 1/4" **ID** clear vinyl | Eastrans 10 ft (Amazon) |
| Thread sealant | yellow gas-rated PTFE | Gasoila (Amazon) |
| Valve seal / antenna | Kapton tape | https://www.amazon.com/dp/B006ROKY68 |

**Thread rule:** yellow PTFE tape on the three **NPT** joints only. The
**9/16-18 LH** regulator connection seals metal-to-metal — **no tape** there.
Wrap NPT clockwise (so threading tightens, not unwinds), 2–3 turns.

---

## 5. Procedure

### 5a. Air leak test (bench only)
Catches gross faults (seams, valve). Does **not** prove H₂ retention (H₂ is smaller).

1. Inflate with the electric air pump to **taut, not drum-tight** (don't stress seams).
2. Tape the valve (Kapton) as backup seal.
3. Measure circumference at widest point; note time.
4. Sit 24–48 h in a **temperature-stable** spot (no window/vent/AC).
5. Re-measure. Held = pass. Sagged = leak (usually valve/seam).
   Optional: soapy water on valve/seams to pinpoint.

### 5b. Pre-stretch
Yokohama/SAG fly better pre-stretched (community "Stretchinator" method).
The Orbs won't fully round on breath/straw alone — needs an electric inflator or
gas regulator. Popping/cracking during inflation = seams separating, **not** failure.
Inflate until wrinkles gone and round.

### 5c. Free-lift measurement (Traquito scale trick)
Target free lift **~5–7 g** (too much bursts at altitude; too little won't climb).

1. Weigh full load (tracker + solar + both dipole legs + tape + ribbon).
2. Pick a reference weight **≥30 g heavier** than the load.
3. `target scale reading = reference weight − (load + free lift)`
4. Set reference weight on scale — **do NOT tare**.
5. Tie balloon to reference weight; fill. Balloon pulls up → reading drops.
6. Stop when scale hits target reading. ±0.5 g is fine.

**Worked example** (substitute measured weights):
```
load (all-up)   = 15 g
free-lift target =  6 g   → needed neck lift = 21 g
reference weight = 60 g    (45 g > load ✓)
target reading   = 60 − (15 + 6) = 39 g  → fill until scale = 39 g
```
Note: balloons lose ~0.5 g of gas overnight — fill correctly and launch promptly.

### 5d. Hydrogen fill — SAFETY
H₂ is flammable across a wide mix range; ignites from static. Foil holds static.
- Fill **outdoors / well-ventilated**, away from any flame, spark, motor.
- Secure cylinder upright. Discharge static (touch grounded metal) first.
- Run regulator delivery pressure **as low as stable** (a couple psi); meter with
  the needle valve, not pressure. Low flow also reduces static.
- Keep tracker electronics off and clear during fill.

### 5e. Seal
- Foil party balloon: tape valve (Kapton) — no heat-seal needed.
- Yokohama/SAG clear-plastic fill port: **heat-seal** (impulse sealer) after fill.
- Attach payload; verify gentle rise (~free-lift target).

---

## 6. Open items

- [ ] **Norco: confirm regulator outlet thread** (assumed 9/16-18 LH "B"). Gates the
      Dixon adapter — bought at-risk pending this.
- [ ] Weigh full payload (0.1 g scale) → lock free-lift numbers.
- [ ] Air pump + remaining fittings arrive → run leak test.
- [ ] Order flight balloon (SAG Orbs 32" Clear) once practice flow validated.
- [ ] Impulse heat sealer (only if flying Yokohama/SAG clear-port balloon).

---

## 7. On hand / ordered

Balloons (PartyWoo 50" foil, practice) · H₂ cylinder · air pump (ordered) ·
1/4" ID tubing (ordered) · needle valve (ordered) · SUNGATOR barb + clamps ·
Gasoila gas PTFE tape (ordered) · Dixon LH adapter (at MSC) · Kapton tape ·
0.1 g scale · Jetpack trackers + solar.

---

## 8. Key references

- Traquito balloons: https://traquito.github.io/flying/balloons/
- Traquito free lift: https://traquito.github.io/flying/freelift/
- K9YO balloon guide: https://sites.google.com/view/picoballoonsbyk9yo/balloon
- NIBBB technical: https://nibbb.org/technical-info/
- Ruthroff flight logs: https://www.theastroimager.com/picoballoning/pico-ballooning/
- Pico community: https://groups.io/g/picoballoon

# Vaillant article number → model map

An open, crowdsourced map from the Vaillant **article number** (Sachnummer) to the
**exact appliance model**.

## Why

On eBUS, the `0704` scan only returns the shared control-board identifier
(`HMU00`, `CTLV3`, `V32`, …). That identifier is the same across many different
appliances — two completely different units can both scan as `V32`. The thing
that actually names the model is the 10-digit **article number**, carried in the
Vaillant extended identification (`b509 24/25/26/27`).

Vaillant does not publish a mapping from that number to the model name. This
repository collects it so that tools reading the bus can resolve the real model
instead of a generic board id.

## Please share only the article number — never your full serial

The full 28-character serial contains a per-unit production counter that
**identifies your individual device**. The **article number alone** is all the
map needs, and it is the same for every unit of the same model. Post the article
number, not the serial.

## Map

| Article number | Model (nameplate) | Source |
|---|---|---|
| `0010021122` | aroTHERM plus VWL 125/6 A S2 | installation A |
| `0010021118` | aroTHERM plus VWL 75/6 A S2 | [john30/ebusd#1765](https://github.com/john30/ebusd/issues/1765) |
| `0010023696` | eloBLOCK VE 24 | installation A |
| `8000015501` | VR 940f (internet gateway) | installation A |
| `0010045478` | sensoCOMFORT VRC 720/2 | printed manual |
| `0010014313` | aguaFLOW exclusive VPM 40/45/2 W | installation A |
| `0020184846` | VR 71 (mixer & solar module) | installation A |
| `0020235465` | VR 32/B (bus coupler) | installation A |
| `0010031643` | VWZ AI (control / interface module) | installation A |

## Observed structure

Help confirm or refine these patterns:

- The **prefix** looks like a product family:
  - `0010…` — heat generators, controls, hydraulics
  - `0020…` — modules & couplers (VR 71, VR 32)
  - `8000…` — connectivity (VR 940f)
- The **last digits** encode the variant within a family. For aroTHERM plus the
  size: `…18` = VWL 75/6, `…22` = VWL 125/6.

These are hypotheses from a small sample — more data points will prove or break
them.

## How to contribute

1. Read `b509 24`, `25`, `26` and `27` from your device and concatenate the ASCII
   payload to the 28-character serial. Its layout is
   `21` + year + week + **10-char article number** + factory mark + counter + `N`
   + check digit. A 6-digit article number is `<`-padded to ten.
2. The article number is **characters 7–16** of the serial.
3. Open an issue (or a pull request editing the table above) with the **article
   number** and the **model printed on your nameplate / invoice**. The serial is
   not needed — please leave it out.

## License & provenance

The entries are plain facts — an article number next to the model on a
nameplate — contributed by owners from their own hardware.

Everything here is released under **[CC0 1.0](LICENSE)** (public domain), so the
map can be embedded freely in any project, including Apache-2.0 licensed ones.

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

The map lives in **[`article-numbers.csv`](article-numbers.csv)** — one row per
entry (`article_number,model,source,notes`). GitHub renders it as a sortable
table when you open it, and the plain CSV drops straight into any tool.

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

**Easiest — open an issue:** use the *"Add an article number"* issue form and fill
in the **article number** and the **model printed on your nameplate / invoice**.
Thirty seconds, no Git needed; we add the row.

**Or send a pull request** adding a line to
[`article-numbers.csv`](article-numbers.csv).

Where to find the article number: it is printed on the nameplate and on the
invoice. The full serial is also shown in the Vaillant app (myVAILLANT /
sensoApp), and the article number is **characters 7–16** of it — but the app does
**not** show the exact model designation (type and power class), so the nameplate
or invoice stays the authoritative source for the model name. Over the bus the
article number is likewise **characters 7–16** of the `b509 24/25/26/27` serial
(layout: `21` + year + week + **10-char article number** + factory mark + counter
+ `N` + check digit; a 6-digit article number is `<`-padded to ten).

Please share only the article number — **leave out your full serial.**

## License & provenance

The entries are plain facts — an article number next to the model on a
nameplate — contributed by owners from their own hardware.

Everything here is released under **[CC0 1.0](LICENSE)** (public domain), so the
map can be embedded freely in any project, including Apache-2.0 licensed ones.

# Fertigungsregeln (4 Lagen) – gemeinsamer Standard AISLER / JLCPCB / Eurocircuits

Ziel: Das Board soll ohne Änderungen und ohne Aufpreis bei allen drei Herstellern bestellt werden können.
Für jeden Parameter gilt deshalb der strengste der drei Werte.

Stand der Herstellerdaten: 02.11.2026. Vor einer Bestellung kurz gegenprüfen, die Hersteller ändern ihre Grenzen gelegentlich.

## Annahmen

- 4 Lagen, 1,6 mm, Kupfer 35 µm, ENIG, grüne Lötstoppmaske
- Nur durchgehende Vias (keine Blind-/Buried-/Micro-Vias)
- AISLER: Prozess „4 Layer 35 µm ENIG“
- JLCPCB: Standard-4-Lagen ohne Aufpreisoptionen
- Eurocircuits: **Pattern-Klasse 6, Bohrklasse C**. Das ist die feinste Kombination, die in *allen* Diensten geht, auch im günstigen PCB proto. Klasse 7/8 und Bohrklasse D gibt es nur in STANDARD pool und den teureren Pools.

**Wichtig zu Eurocircuits:** KiCad rechnet mit dem Endmaß der Bohrung. Eurocircuits rechnet Restring und Bohrabstand ab Werkzeug, und das Werkzeug ist bei durchkontaktierten Bohrungen 0,10 mm größer als das Endmaß. Die Werte unten sind schon umgerechnet.
Beispiel: Via 0,25 / 0,60 mm → Werkzeug 0,35 mm → Restring nach Eurocircuits (0,60 − 0,35) / 2 = 0,125 mm (Klasse 6) → in KiCad (0,60 − 0,25) / 2 = 0,175 mm.

## Vergleich und gewählter Wert

| Parameter | AISLER | JLCPCB | Eurocircuits 6C | **Gewählt (KiCad)** | Bestimmt durch |
|---|---|---|---|---|---|
| Leiterbahnbreite | 0,125 | 0,09 | 0,150 | **0,15** | EC |
| Kupferabstand | 0,125 | 0,09 (Pad–Pad 0,15) | 0,150 | **0,15** | EC, JLC |
| Via-Bohrung min. (Endmaß) | 0,25 (max. 0,45) | 0,15 (< 0,3 nur aufpreisfrei mit Ø ≥ 0,45) | 0,25 | **0,25** | AISLER, EC |
| Via-Durchmesser min. | 0,45 | 0,45 (aufpreisfrei) | 0,60 bei 0,25 Bohrung | **0,60** | EC |
| Via-Restring (KiCad) | 0,10 | 0,15 | 0,125 + 0,05 = 0,175 | **0,175** | EC |
| Bauteilbohrung Restring | 0,30 | 0,15–0,20 | 0,175 | **0,30** | AISLER |
| Bohrung zu Bohrung | 0,30 | 0,20 (Via), 0,45 (Pad) | 0,25 ab Werkzeug = 0,35 | **0,35 / Pad 0,45** | EC, JLC |
| Bohrung zu fremdem Kupfer | 0,25 (NPTH) | 0,20 (Via), 0,28–0,30 (PTH) | IPI 0,20 ab Werkzeug = 0,25 | **0,25 / Pad 0,30** | EC, JLC |
| NPTH min. | 0,50 | 0,50 | 0,35 | **0,50** | AISLER, JLC |
| Bohrung max. | 5,6 | 6,3 | – | **5,6** | AISLER |
| Kupfer zum Rand außen | 0,30 | 0,20 | 0,25 | **0,30** | AISLER |
| Kupfer zum Rand innen | 0,30 | 0,20 | 0,40 | **0,40** | EC |
| Lötstopp-Steg min. | 0,10 | 0,10 (grün) | 0,075 | **0,10** | AISLER, JLC |
| Lötstopp-Erweiterung | 0 (macht AISLER selbst) | 1:1 | macht EC selbst | **0** | AISLER |
| Silk Linienbreite | 0,15 | 0,15 | 0,10 | **0,15** | AISLER, JLC |
| Silk Texthöhe | 0,80 | 1,00 | 1,00 | **1,00** | JLC, EC |
| Silk zu Pad | 0,125 | 0,15 | 0,10 | **0,15** | JLC |
| Silk zum Rand | – | – | 0,25 | **0,25** | EC |

Alle Maße in mm.

## Wo die Regeln in KiCad stehen

- **Board-Setup → Constraints / Netzklassen** (`phyphox-mini-pcb-v2.kicad_pro`): Grundwerte aus der Tabelle, vordefinierte Leiterbahnbreiten (0,15 … 0,5) und Vias (0,6/0,25 · 0,65/0,3 · 0,8/0,4). Netzklasse Default: 0,15 Abstand, 0,2 Bahn, Via 0,6/0,25.
- **Board-Setup → Lagenaufbau** (`.kicad_pcb`): 4 Lagen nach AISLER, Lötstopp-Erweiterung 0, Mindeststeg 0,1.
- **Benutzerdefinierte Regeln** (`phyphox-mini-pcb-v2.kicad_dru`): alles, was die Constraints-Seite nicht kann: Innenlagen-Randabstand, Bauteilbohrungen, NPTH, Via max. 0,45, Silk zum Rand, keine Blind-/Micro-Vias, keine Lötstopp-Overrides an Pads.

## Lagenaufbau (AISLER 4L 1,6 mm)

| Lage | Material | Dicke | εr |
|---|---|---|---|
| F.Cu | Kupfer | 35 µm | |
| Prepreg | Panasonic R-1551(W) | 0,138 mm | 4,3 |
| In1.Cu | Kupfer | 35 µm | |
| Kern | Panasonic R-1566(W) | 1,113 mm | 4,6 |
| In2.Cu | Kupfer | 35 µm | |
| Prepreg | Panasonic R-1551(W) | 0,138 mm | 4,3 |
| B.Cu | Kupfer | 35 µm | |

Die anderen Hersteller haben andere Aufbauten (JLC04161H-7628: Prepreg 0,21 mm, Innenkupfer 0,5 oz; Eurocircuits PCB proto: 1,55 mm). Für die DRC ist das egal. **Für die 50-Ω-Antennenzuleitung aber nicht:** Die Breite hängt vom Abstand F.Cu → In1.Cu ab und muss für den tatsächlich gewählten Hersteller berechnet werden (AISLER-Angabe: 50 Ω Microstrip auf Top ≈ 0,295 mm).

## Quellen

- AISLER 4 Layer 35 µm ENIG Design Rules: https://community.aisler.net/t/4-layer-35-m-enig-design-rules/3733
- AISLER 4-Lagen-Stackup: https://community.aisler.net/t/4-layers-1-6mm-35-m-stackup/5457
- AISLER KiCad-Vorlage/Regeln: https://github.com/AislerHQ/aisler-support/tree/master/kicad/aisler-4-layer-hd-drc
- JLCPCB Capabilities: https://jlcpcb.com/capabilities/pcb-capabilities
- JLCPCB Stackups: https://jlcpcb.com/impedance
- Eurocircuits Klassifizierung (Tabelle 07/05/2024): https://www.eurocircuits.com/pcb-design-guidelines/classification/
- Eurocircuits Bohrungen: https://www.eurocircuits.com/technical-guidelines/pcb-design-guidelines/drilled-holes/
- Eurocircuits Kupferlagen / Randabstand: https://www.eurocircuits.com/technical-guidelines/pcb-design-guidelines/copper-layers/
- Eurocircuits Lötstopp: https://www.eurocircuits.com/technical-guidelines/pcb-design-guidelines/soldermask/
- Eurocircuits Bestückungsdruck: https://www.eurocircuits.com/technical-guidelines/pcb-design-guidelines/legend-print/
- Eurocircuits Dienste: https://www.eurocircuits.com/services/pcb-proto/ , https://www.eurocircuits.com/services/standard-pool/

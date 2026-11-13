# Projektbibliothek `phyphox-mini`

Enthält nur Teile, die es in den KiCad-Standardbibliotheken nicht gibt.
Eingebunden über `sym-lib-table` / `fp-lib-table` mit `${KIPRJMOD}`.

| Datei | Inhalt | Herkunft |
|---|---|---|
| `phyphox-mini.kicad_sym` → `nRF54L15-QFAA` | Schaltplansymbol | Eigene Erstellung; Pinbelegung aus dem nRF54L15-Datenblatt (QFN48 pin assignments) |
| `phyphox-mini.pretty/nRF54L15-QFAA_QFN-48-1EP_6x6mm_P0.4mm` | Footprint | Eigene Erstellung; Maße (Pads 0,2 × 0,65 mm @ 3,025 mm, EP 4,7 mm) nach Nordic nRF54L15-QFAA Reference Layout v0.8; 16 Masse-Vias im EP (4×4, Raster 1,2 mm, 0,25/0,6 mm) wie im Referenzlayout |
| `phyphox-mini.3dshapes/nRF54L15-QFAA_QFN-48-1EP_6x6mm_P0.4mm.step` | 3D-Modell | Eigene Erstellung mit CadQuery (6 × 6 × 0,85 mm, P 0,4 mm, EP 4,6 mm) |
| `phyphox-mini.kicad_sym` → `BMI423` | Schaltplansymbol | Abgeleitet vom KiCad-Symbol `Sensor_Motion:BMI160` (gleiche LGA-14-Belegung); Pins 2/3/10/11 als NC nach BMI423-Datenblatt (BST-BMI423-DS000) |
| `phyphox-mini.kicad_sym` → `BMP581` | Schaltplansymbol | Eigene Erstellung; Pinbelegung aus BST-BMP581-DS004 |
| `phyphox-mini.kicad_sym` → `STCC4` | Schaltplansymbol | Eigene Erstellung; Pinbelegung aus Sensirion STCC4-Datenblatt D1 |
| `phyphox-mini.pretty/BMP581_LGA-10_2x2mm_P0.5mm` | Footprint | Eigene Erstellung nach BST-BMP581-DS004 (Land Pattern: Package + 25 µm); ohne Lötstopp unter dem Sensor, Keepout für Leiterbahnen/Vias auf F.Cu im Bereich innerhalb des Padrings (±0,45 mm) |
| `phyphox-mini.pretty/STCC4_LGA-8_3x4mm` | Footprint | Eigene Erstellung nach STCC4-Datenblatt D1 (Pads 0,85 × 0,85 mm, Pin 1 mit Fase) |
| `phyphox-mini.pretty/Sensirion_DFN-4_1.5x1.5mm_P0.8mm_SHT4x_NoCentralPad` | Footprint | Kopie des KiCad-Footprints gleichen Namens, nur das 3D-Modell zeigt auf das eigene Modell (KiCad liefert dafür keins mit) |
| `phyphox-mini.pretty/Texas_SWRA117D_2.4GHz_Right_Pad2-0.65mm` | Footprint | Kopie des KiCad-Footprints `RF_Antenna:Texas_SWRA117D_2.4GHz_Right`; Pad 2 (Massebohrung) von 0,9 × 0,5 auf 0,9 × 0,65 mm vergrößert, damit der Restring 0,175 mm erreicht (Fertigungsregel) |
| `phyphox-mini.pretty/C_0201_0603Metric_Nordic`, `L_0201_0603Metric_Nordic`, `R_0201_0603Metric_Nordic` | Footprints | Eigene Erstellung; Padmaße (0,27 × 0,30 mm, Raster 0,57 mm) nach Nordic nRF54L15-QFAA Reference Layout v0.8; 3D-Modelle aus den KiCad-Bibliotheken (nur referenziert) |
| `phyphox-mini.pretty/L_0603_1608Metric_Nordic` | Footprint | Eigene Erstellung; Padmaße (0,75 × 0,80 mm, Raster 1,35 mm) nach Nordic nRF54L15-QFAA Reference Layout v0.8; 3D-Modell aus den KiCad-Bibliotheken (nur referenziert) |
| `phyphox-mini.3dshapes/BMP581_LGA-10_2x2mm.step` | 3D-Modell | Eigene Erstellung mit CadQuery (2,0 × 2,0 × 0,75 mm) |
| `phyphox-mini.3dshapes/STCC4_LGA-8_3x4mm.step` | 3D-Modell | Eigene Erstellung mit CadQuery (3,0 × 4,0 × 1,17 mm) |
| `phyphox-mini.3dshapes/SHT4x_DFN-4_1.5x1.5mm.step` | 3D-Modell | Eigene Erstellung mit CadQuery (1,5 × 1,5 × 0,5 mm) |

Diese Dateien fallen unter die Lizenz des Projekts. Ausnahme sind die aus den
KiCad-Bibliotheken abgeleiteten Elemente: die Footprints
`Sensirion_DFN-4_1.5x1.5mm_P0.8mm_SHT4x_NoCentralPad` und
`Texas_SWRA117D_2.4GHz_Right_Pad2-0.65mm` sowie das Symbol `BMI423`. Als
Bibliothekselemente bleiben sie unter CC-BY-SA 4.0
(© KiCad Library Team, https://gitlab.com/kicad/libraries).

Alle übrigen Symbole, Footprints und 3D-Modelle stammen aus den offiziellen
KiCad-Bibliotheken (CC-BY-SA 4.0 mit Ausnahme: Designs, die diese Bibliotheken
verwenden, dürfen unter beliebiger Lizenz – auch kommerziell – veröffentlicht werden;
siehe https://www.kicad.org/libraries/license/).

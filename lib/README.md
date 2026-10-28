# Projektbibliothek `phyphox-mini`

Enthält nur Teile, die es in den KiCad-Standardbibliotheken nicht gibt.
Eingebunden über `sym-lib-table` / `fp-lib-table` mit `${KIPRJMOD}`.

| Datei | Inhalt | Herkunft |
|---|---|---|
| `phyphox-mini.kicad_sym` → `nRF54L15-QFAA` | Schaltplansymbol | Eigene Erstellung; Pinbelegung aus dem nRF54L15-Datenblatt (QFN48 pin assignments) |
| `phyphox-mini.pretty/nRF54L15-QFAA_QFN-48-1EP_6x6mm_P0.4mm` | Footprint | Eigene Erstellung; Maße (Pads 0,2 × 0,65 mm @ 3,025 mm, EP 4,7 mm) nach Nordic nRF54L15-QFAA Reference Layout v0.8 |
| `phyphox-mini.3dshapes/nRF54L15-QFAA_QFN-48-1EP_6x6mm_P0.4mm.step` | 3D-Modell | Eigene Erstellung mit CadQuery (6 × 6 × 0,85 mm, P 0,4 mm, EP 4,6 mm) |

Diese Dateien fallen unter die Lizenz des Projekts.

Alle übrigen Symbole, Footprints und 3D-Modelle stammen aus den offiziellen
KiCad-Bibliotheken (CC-BY-SA 4.0 mit Ausnahme: Designs, die diese Bibliotheken
verwenden, dürfen unter beliebiger Lizenz – auch kommerziell – veröffentlicht werden;
siehe https://www.kicad.org/libraries/license/).

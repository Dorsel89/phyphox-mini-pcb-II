# CE-/EMV-Review phyphox-mini-pcb-v2

Stand: 09.11.2026, geprüft wurde der Commit `b9a4381` („changed battery connector“).
Ziel: Verbesserungen am Layout vor dem ersten Prüflabor-Termin (CE nach RED), Fertigung 4 Lagen bei AISLER.
An den Designdateien wurde nichts geändert, dieses Dokument enthält nur Vorschläge.

## 1. Kurzfassung

Das Board ist sorgfältig gemacht: Die Nordic-Referenzschaltung ist bauteilgenau übernommen, die Antenne ist maßgetreu, die Sperrfläche unter der Antenne gilt auf allen vier Lagen, ein Pi-Netzwerk und eine Lötfläche für ein Messkabel sind vorhanden. Die folgenden Punkte würde ich vor der Zertifizierung trotzdem angehen:

| Nr. | Priorität | Maßnahme |
|---|---|---|
| A1 | hoch | In2 von VDD-Fläche auf Masse umstellen (wie Nordic-Referenz), VDD als Leiterbahnen führen |
| A2 | hoch | Innenlagen-Aussparung unter der Anpassung 1:1 aus dem Nordic-Layout übernehmen; sie beginnt bei dir ca. 0,5 mm zu spät, der C6-Knoten liegt über Masse |
| A3 | hoch | Antenne im Endzustand (Knopfzelle, Gehäuse, Trageband) mit VNA messen und abstimmen; Sendeleistung an die PSD-Grenze anpassen |
| C1 | hoch | Batteriekontakt-Konzept klären: Signal-/VDD-Vias und EP-Vias unter der Zelle, Plus-Kontakt TP11 direkt am Antennenende |
| E1 | hoch | Hersteller und Lagenaufbau für das Zertifizierungsmuster festschreiben; ein Wechsel des Fertigers ändert die HF-Eigenschaften |
| A4 | mittel | nRESET-Leiterbahn auf B.Cu von den C9-Massevias wegnehmen; C9-Masse als Kupferinsel mit 3 Vias |
| B1 | mittel | Masse-Vias am Platinenrand auf ca. 2,5 mm Raster verdichten, ein Via rechts neben der Antennenzuleitung ergänzen |
| C2 | mittel | ESD: Testpunkte und Sensoröffnungen im Gehäusekonzept absichern |
| C3 | mittel | Kein Metall im 3,2-mm-Loch über der Antenne (Schlüsselring, Metallschraube) |
| D1 | mittel | Lotpasten-Fenster des nRF54L15-Mittelpads liegen über offenen Vias |
| E2 | mittel | Kennzeichnung: Typ, Revision, Seriennummer, Batteriepolung auf Silk/Gehäuse vorsehen |
| D2–D5, B2, B3 | niedrig | DRC-Fehler an TP11, veraltete Stückliste, HF-Leiterbahnbreite, Sonstiges |

## 2. Grundlage und Verlässlichkeit der Aussagen

Untersucht wurden `phyphox-mini-pcb-v2.kicad_pcb`, `.kicad_sch`, `.kicad_pro`, `.kicad_dru`, die Stückliste und `docs/fertigungsregeln.md`. Die Geometrie habe ich mit der KiCad-10-Python-Schnittstelle ausgelesen (nur lesend), DRC und ERC über `kicad-cli` laufen lassen und alle vier Kupferlagen geplottet. Koordinaten in diesem Dokument sind KiCad-Platinenkoordinaten in mm.

Die Herstellerangaben stammen aus einer Web-Recherche vom 09.11.2026. Nicht alles ließ sich an der Primärquelle bestätigen. Deshalb steht hinter jeder Aussage, worauf sie beruht:

- **[Layout]** – von mir direkt an den Designdateien gemessen oder gesehen.
- **[Referenz]** – am Nordic-Referenzlayout nRF54L15-QFAA v0.8 abgelesen (PDF der Lagen gerendert und ausgemessen, Genauigkeit ca. ±0,05 mm).
- **[Quelle]** – in der Recherche an der Primärquelle gelesen.
- **[Sekundär]** – Antwort von Nordic-Mitarbeitern im DevZone-Forum oder Suchtreffer, nicht an einem Datenblatt bestätigt.
- **[unbestätigt]** – Fachwissen ohne Quellenprüfung in dieser Sitzung. Vor einer Entscheidung gegenprüfen.

## 3. Was bereits passt

- HF-Bauteile entsprechen exakt der Nordic-Stückliste v0.8: C6 1,5 pF, C9 2,0 pF, C11 0,3 pF, C13 3,9 pF, L2 2,7 nH, L3/L4 3,5 nH, C5 2,2 nF, C12 10 nF, FB1, L1 4,7 µH. [Quelle]
- Die beiden Nordic-Pflichtregeln sind umgesetzt: C6-Masse nur an Pin 32, C9-Masse nur zur unteren Lage (Sperrflächen „C6 GND nur an Pin 32“ und „C9 GND nur Bottom“). [Layout]
- Antenne: Die Maße des Footprints stimmen mit Tabelle 1 der TI AN043 überein (L1 3,94 … L6 4,90, W1 0,90, W2 0,50). Die Massekante liegt bei y = 75,6 und damit 0,47 mm unter dem offenen Antennenende; TI gibt D4 = 0,5 mm vor. [Layout, Quelle]
- Antennen-Sperrfläche auf F.Cu, In1, In2 und B.Cu, kein Kupfer und kein Bestückungsdruck im Antennenbereich. [Layout]
- Pi-Netzwerk C22 – R2 – C21 an der Zuleitung, Lötfläche „RF-GND: Loetflaeche Messkabel“ direkt daneben. Damit sind Abstimmung und leitungsgebundene Messung möglich. [Layout]
- Gesamtabstand F.Cu → B.Cu ist bei AISLER (ca. 1,46 mm) praktisch gleich dem Nordic-Aufbau (ca. 1,47 mm). Da der größte Teil der Anpassung über der Aussparung liegt und sich auf die untere Lage bezieht, weicht der AISLER-Aufbau dort kaum von der Referenz ab. [Layout, Referenz]
- Zuleitungsbreite 0,3 mm entspricht der AISLER-Angabe für 50 Ω (0,295 mm, Top gegen In1). [Quelle]
- In1 ist eine durchgehende Massefläche, DRC ohne unverbundene Netze, Schaltplan und Platine sind synchron. [Layout]
- UART-Testpunkte TP8/TP9 für den Direct Test Mode und SWD-Testpunkte sind vorhanden.

## 4. Befunde und Empfehlungen

### A. HF-Pfad und Antenne

**A1 – In2 ist eine VDD-Fläche, in der Nordic-Referenz ist Lage 3 Masse (hoch)**

Im Referenzlayout sind beide Innenlagen Masseflächen; die Masse-Vias sind auf Lage 2 und Lage 3 ohne Freistellung angebunden. [Referenz] Bei dir ist In2 eine vollflächige VDD-Zone (271 mm²) mit derselben Aussparung unter der Anpassung. [Layout]

Folgen:
- Die Felder der Anpassung greifen durch die Aussparung bis zur unteren Lage. An den Kanten der Aussparung liegt bei dir auf In2 nicht Masse, sondern VDD. HF (Grundwelle und Oberwellen) koppelt damit direkt auf die Versorgungsfläche, die über die ganze Platine reicht und an der Batterie, allen Sensoren und den Pads TP1/TP11 hängt.
- Die VDD-Fläche bildet mit der Masse auf B.Cu (Abstand 0,138 mm) einen Hohlraumresonator von rund 20 mm Kantenlänge. Seine Resonanzen liegen im Bereich der Oberwellen (ab ca. 3,6 GHz). [unbestätigt, Abschätzung]
- Signale auf B.Cu haben VDD als Bezugslage; beim Lagenwechsel von F.Cu nach B.Cu wechselt der Rückstrom von Masse auf VDD. Das betrifft auch die DECA-Verbindung (5,7 mm auf B.Cu).
- Der Nutzen der VDD-Fläche ist gering: Die Plattenkapazität gegen B.Cu beträgt nur etwa 75 pF.

Empfehlung: In2 als Massezone anlegen. VDD mit 0,3–0,5 mm breiten Leiterbahnen oder kleinen lokalen Flächen führen (13 VDD-Vias sind betroffen). Wenn eine VDD-Fläche bleiben soll, dann nur lokal im Sensorbereich rechts unten und mit mindestens 3 mm Abstand zur Anpassung und zur Antennenkante.

**A2 – Aussparung unter der Anpassung weicht von der Referenz ab (hoch)**

Deine Sperrfläche „HF-Aussparung Innenlagen (Nordic)“ ist ein Rechteck x 74,10 … 76,45, y 79,30 … 82,75. [Layout] Die Referenz hat eine abgestufte Form. [Referenz]

| Stelle | Nordic-Referenz | Dein Layout |
|---|---|---|
| Beginn (IC-Seite) | zwischen den beiden Pads von L2, ca. 0,15 mm vor dem C6-Knoten | bei y = 82,75, also erst hinter dem C6-Knoten (Pads bei y = 82,99) |
| C6-Knoten (L2 Pad 2, C6 Pad 1) | liegt über der Aussparung, mit eigener Ausbuchtung zur C6-Seite | liegt über In1-Masse in 0,138 mm Abstand |
| Abschnitt L3 | ca. 0,8 mm breit, mittig zum Pfad | 2,35 mm breit, einseitig nach rechts |
| C9-Masse | eigene Tasche um die Masse-Vias | im Rechteck enthalten |
| Ende | läuft bis zum 50-Ω-Punkt durch | endet bei y = 79,3 kurz hinter C11 |

Wichtig ist die erste Zeile. Über Masse hat der C6-Knoten etwa 0,06–0,1 pF zusätzliche Kapazität gegen Masse (Abschätzung aus Padfläche und Prepregdicke). Nordic schreibt für C6 eine Toleranz von ±0,05 pF vor; die Abweichung liegt also in derselben Größenordnung wie die Bauteiltoleranz, die Nordic für nötig hält. C6 ist der erste Querkondensator des Oberwellenfilters.

Außerdem liegt das Masse-Via bei (75,43 / 82,50) innerhalb der Aussparung und ist deshalb nur an F.Cu und B.Cu angebunden.

Empfehlung: Die Kontur nicht nachzeichnen, sondern übernehmen. `nRF54L15_qfaa_4L_config1.PcbDoc` aus dem Referenzlayout-Paket lässt sich in KiCad importieren (Datei → Importieren → Nicht-KiCad-Platine). Die Aussparungen von Lage 2 und 3 dann relativ zu Pin 31 auf In1 und In2 übertragen. Als Anhaltswert: Beginn bei etwa y = 83,27 (Mitte zwischen den Pads von L2).

Hinweis zur Quellenlage: Im DevZone-Forum schreiben Nordic-Mitarbeiter, man solle die Innenlagen-Masse unter der Anpassung möglichst erhalten und nur um die C9-Vias freistellen. [Sekundär] Das veröffentlichte Layout v0.8 zeigt aber eindeutig die oben beschriebene Aussparung. Maßgeblich ist das Layout. Prüfe vor der Übernahme, ob es inzwischen eine neuere Version als v0.8 gibt; das Blatt trägt den Vermerk, dass sich das Layout noch ändern kann.

**A3 – Antenne muss im Endzustand gemessen werden (hoch)**

Die AN043-Antenne ist für die Platine des CC2511-USB-Dongles entworfen. TI empfiehlt ausdrücklich dieselbe Platinendicke und dasselbe Material wie im Referenzdesign und nennt als Ausgleich für andere Dicken eine etwas kürzere oder längere Antenne. Die Referenzdicke steht nicht in der Application Note. [Quelle] Bei dir weichen mehrere Dinge ab:

- Die Massefläche ist mit ca. 20 × 21 mm klein. TI misst auf dem Dongle etwa 250 MHz Bandbreite im Freiraum und nur noch etwa 100 MHz an einem Laptop; die Massegröße wirkt sich also stark aus. [Quelle]
- TI hat die Resonanz freistehend bei 2,54 GHz gemessen, also oberhalb des Bandes. Ein Kunststoffgehäuse zieht sie nach unten. [Quelle]
- Die Knopfzelle (20 mm Metallscheibe) reicht bis ca. 0,6 mm an die Massekante heran, wenn sie mittig zum unteren Kreisbogen sitzt (Mittelpunkt 77,24 / 86,20). [Layout]
- Über der Antenne stehen noch 5,8 mm FR4 mit dem 3,2-mm-Loch.

Empfehlung:
1. S11 an der Lötfläche messen, mit eingelegter Zelle, im Gehäuse und mit Trageband, zusätzlich mit der Hand am Gerät. Das Koaxkabel selbst verschiebt die Resonanz um 5–10 MHz. [Quelle]
2. Erst die Geometrie, dann das Pi-Netzwerk: Liegt die Resonanz deutlich daneben, in der nächsten Revision die Länge des letzten Antennenarms anpassen und das Pi-Netzwerk nur für den Rest verwenden.
3. Keine Metallteile (Batterieclip, Feder, Schraube) über die Massekante y = 75,6 hinaus in den Antennenbereich ragen lassen.
4. Sendeleistung festlegen: Der nRF54L15 im QFN-Gehäuse liefert maximal +7 dBm (Recherche am Datenblatt, nur teilweise lesbar). EN 300 328 begrenzt für BLE die Leistungsdichte auf 10 dBm/MHz EIRP. Bei 1-Mbit-BLE ist das praktisch die Grenze für die Gesamtleistung. Mit +7 dBm und mehr als etwa 3 dBi Antennengewinn wird es knapp; die Sendeleistung in der Firmware nach der Gewinnmessung festlegen. [unbestätigt, Grenzwert vor dem Labortermin prüfen]

**A4 – nRESET-Leiterbahn auf B.Cu direkt neben den C9-Massevias (mittel)**

Die C9-Masse geht über die Vias (76,00 / 80,60) und (76,00 / 81,34) nur auf B.Cu. Genau daneben verläuft auf B.Cu die nRESET-Leiterbahn bei x = 76,60 von y = 82,55 bis 80,65. Zwischen Via-Rand und Freistellung der Leiterbahn bleiben rechnerisch weniger als 0,1 mm Kupfer. [Layout] In der Referenz ist die untere Lage im Umkreis von gut 2 mm um die C9-Vias geschlossen. [Referenz]

Empfehlung:
- nRESET hinter R1 so führen, dass B.Cu unter und neben der Anpassung geschlossen bleibt (mindestens 1,5–2 mm Abstand zu den C9-Vias), zum Beispiel das Via weiter nach rechts setzen.
- C9-Massepad wie in der Referenz über eine kleine Kupferinsel mit 3 Vias anbinden, nicht über 0,25–0,3 mm breite Leiterbahnen mit 2 Vias. Laut Nordic sollen diese Verbindungen breit sein. [Sekundär]

**A5 – HF-Leiterbahnen: Impedanz und Abstand zur Massefläche (niedrig)**

Die Abschnitte `/RF` und `/ANT_FEED` sind 0,3 mm breit mit 0,2 mm Abstand zur Massefläche auf F.Cu. [Layout] Nach meiner Abschätzung ergibt das mit Prepreg 0,138 mm und εr 4,3 eher 44–47 Ω als 50 Ω (Mikrostreifen-Näherung, Lötstopplack und seitliche Masse senken zusätzlich). [unbestätigt] Bei 5,5 mm Gesamtlänge ist der Einfluss klein und geht in der Antennenabstimmung auf. Ändern würde ich das nur, wenn die Leiterbahnen ohnehin angefasst werden: Abstand zur Massefläche für die Netzklasse `HF` auf 0,3–0,4 mm erhöhen. AISLER veröffentlicht keine Dk-Toleranz und keinen Verlustfaktor und gibt Empfehlungen für koplanare Leitungen nur auf Anfrage. [Quelle]

### B. Massekonzept und EMV

**B1 – Masse-Vias am Rand und an der Antennenkante (mittel)**

- Entlang der Antennenkante (y ≈ 76,25) stehen die Vias rechts im Raster 1,5–2,0 mm. Um die Zuleitung herum ist die Lücke 3,65 mm groß, von (72,10 / 76,79) bis (75,75 / 76,90). Ein zusätzliches Via bei etwa (75,0 / 76,2) schließt sie.
- Am übrigen Platinenrand liegen die Masse-Vias 4–7 mm auseinander. [Layout]

Empfehlung: Am Rand auf etwa 2,5 mm verdichten, das sind rund 12–15 zusätzliche Vias. Bei 7,3 GHz (3. Oberwelle) entspricht das ungefähr einem Zehntel der Wellenlänge im FR4. Wenn In2 Masse wird (A1), verbinden diese Vias alle vier Masselagen am Rand.

**B2 – Sperrfläche am SHT40 auf allen vier Lagen (niedrig)**

Unter dem SHT40 fehlt auf 2,4 × 2,4 mm jede Bezugslage; die I2C-Leitungen kreuzen diesen Bereich, und SCL läuft auf B.Cu 0,15 mm an der Kante entlang. [Layout] Für I2C ist das unkritisch. Für die thermische Entkopplung reicht es meist, F.Cu und In1 freizustellen und die tieferen Lagen geschlossen zu lassen; das hält die Massefläche ganz.

**B3 – Serienwiderstand in der SPI-Taktleitung (niedrig)**

Laut Errata der ersten Chiprevision können GPIOs abstrahlen; als Abhilfe werden Serienwiderstände, kurze Leitungen und langsame Flanken genannt. [Sekundär, Chiprevision prüfen] Ein 0201-Platz für 22–47 Ω in `IMU_SCK` nahe am nRF54L15 kostet wenig und lässt sich mit 0 Ω bestücken, falls er nicht gebraucht wird. In der Firmware die normale Treiberstärke verwenden.

### C. Batterie, ESD und Produktsicherheit

**C1 – Batteriekontakte TP10/TP11 (hoch, Konzept klären)**

Seit dem letzten Commit gibt es keinen Batteriehalter mehr auf der Platine, sondern zwei Flächen auf B.Cu: TP10 (Masse, Ø 4 mm, Mitte) und TP11 (VDD, 3 × 3 mm, bei 84,03 / 78,53). Wie die Zelle gehalten und kontaktiert wird, geht aus den Dateien nicht hervor. Je nach Konzept sind diese Punkte kritisch:

1. Alle 36 Signal- und VDD-Vias liegen im Bereich der Zelle auf B.Cu. AISLER führt abgedeckte Vias in den Design-Regeln als nicht unterstützt. [Quelle] Liegt die Zelle oder ein Kontaktblech auf der Platine auf, isoliert nur der Lötstopplack. Das Gehäuse der CR2032 ist der Pluspol, und sein Bördelrand liegt auf der Minus-Seite fast in der Ebene der Minus-Fläche. Ein Kurzschluss einer Lithiumzelle ist auch ein Sicherheitsthema bei der Prüfung nach EN 62368-1.
   Empfehlung: Zelle mit Abstand zur Platine lagern (Halter, Isolierfolie oder Gehäusesteg) oder im Auflagebereich keine Vias fremder Netze setzen.
2. Fünf der 16 Masse-Vias des nRF54L15-Mittelpads liegen in der Fläche von TP10, ein weiteres am Rand. [Layout] Beim Löten zieht Lot durch die offenen Bohrungen auf die vergoldete Kontaktfläche (siehe D1). Für einen Druckkontakt oder ein aufgelötetes Kontaktblech ist das ungünstig. TP10 verschieben oder die Vias dort weglassen.
3. TP11 liegt direkt unter dem offenen Ende der Antenne (Antennenarm bei x 84,5 … 85,0, TP11 ab y ≈ 76,4). Ein dort aufgelöteter Metallclip liegt auf VDD und steht 1–3 mm neben der Stelle mit der höchsten Feldstärke. Das verstimmt die Antenne und koppelt HF auf VDD.
   Empfehlung: Plus-Kontakt auf die antennenferne Seite legen (y > 90). Falls das mechanisch nicht geht, direkt an TP11 einen 100-nF-Kondensator und einen kleinen HF-Kondensator (einige pF) gegen Masse setzen und die Antenne mit montiertem Clip abstimmen.
4. Verpolung: Prüfen, was passiert, wenn die Zelle falsch herum eingelegt wird. Es gibt keinen Verpolschutz. [Layout]

**C2 – ESD (mittel)**

EN 301 489-1/-17 verlangt eine ESD-Prüfung nach EN 61000-4-2; üblich sind ±4 kV Kontakt- und ±8 kV Luftentladung. [unbestätigt, Pegel beim Labor erfragen] Auf der Platine gibt es keine Schutzdioden. Die Testpunkte TP2–TP9 führen direkt auf GPIOs, nRESET ist nur über 1 kΩ und 3,9 pF entkoppelt. [Layout]

Das ist vertretbar, wenn das Gehäuse es abfängt:
- Testpunkte dürfen im geschlossenen Gerät nicht berührbar sein. Der Weg von jeder Gehäusefuge bis zum nächsten Leiter sollte lang sein (Faustregel etwa 1 mm je kV).
- Sensoröffnungen (BMP581, STCC4, SHT40) so legen, dass eine Entladung zuerst Masse trifft. TP6/TP7 (I2C) und TP1 (VDD) liegen nahe an den Sensoren und am Rand.
- Die Batteriekontakte sind unkritisch: Eine Entladung auf VDD wird von C20 (47 µF) aufgenommen.
- Firmware: Watchdog und Brown-out-Reset aktivieren. Nach einer Entladung darf die Verbindung abreißen, das Gerät muss sich aber selbst erholen.

**C3 – Loch über der Antenne (mittel)**

Das 3,2-mm-Loch bei (77,24 / 67,95) liegt 2,7 mm über der obersten Antennenleiterbahn. Ein metallischer Schlüsselring oder eine Metallschraube an dieser Stelle verstimmt die Antenne stark. Nur Kunststoff oder Textil vorsehen, das in der Anleitung festhalten und die Antenne mit dem vorgesehenen Befestigungsteil messen.

**C4 – Knopfzellensicherheit (Gehäuse)**

Für Knopfzellen verlangt EN 62368-1 nach meiner Kenntnis ein Batteriefach, das nur mit Werkzeug oder mit zwei unabhängigen, gleichzeitigen Handgriffen zu öffnen ist, dazu einen Warnhinweis. [unbestätigt, Normtext beschaffen oder Labor fragen] Die Batterieverordnung (EU) 2023/1542 verlangt ab 18.02.2027, dass Endnutzer die Batterie selbst tauschen können; ein dafür nötiges Werkzeug muss handelsüblich sein. [Quelle] Beides zusammen spricht für einen Deckel mit normaler Schraube. Die Schraube darf nicht im Antennenbereich sitzen.

### D. AISLER-Fertigung und Bestückung

**D1 – Lotpaste über offenen Vias im Mittelpad des nRF54L15 (mittel)**

Der Footprint hat 3 × 3 Pastenfenster von 1,25 mm im Raster 1,55 mm und 4 × 4 Vias im Raster 1,2 mm. Alle 16 Vias liegen ganz oder teilweise unter einem Pastenfenster. [Layout] AISLER verschließt Vias nicht. [Quelle] Lot läuft beim Reflow in die Bohrungen und tritt unten aus; dem Mittelpad fehlt dann Lot.

Empfehlung: Pastenfenster zwischen die Via-Reihen legen (zum Beispiel kleinere Fenster auf den Positionen 0 und ±1,2 mm) und 40–60 % Pastenbedeckung anstreben. Die Via-Bohrung von 0,25 mm begrenzt das Problem, behebt es aber nicht.

**D2 – DRC-Fehler an TP11 (niedrig)**

- Abstand TP11 (VDD) zum Masse-Via (83,40 / 76,25): 0,141 mm statt 0,15 mm.
- Der Bestückungsdruck von TP11 ragt über die Platinenkante.

Via oder Pad um 0,05 mm verschieben, Bestückungsdruck an TP11 entfernen. ERC meldet ein 0,0127 mm langes loses Leitungsstück nahe dem Blattursprung; löschen.

**D3 – Stückliste ist veraltet (niedrig)**

`BOM-phyphox-mini-pcb_v2.csv` enthält noch BT1 (Keystone 3034), den es in Schaltplan und Platine nicht mehr gibt. D1 hat keine Herstellernummer. Vor der Bestellung neu exportieren. Für die technische Dokumentation zur CE-Erklärung muss die Stückliste zum geprüften Muster passen.

**D4 – Regeln und Bestückung bei AISLER (Information)**

- Deine Regeln (0,15 mm Bahn und Abstand, Via 0,6/0,25) liegen innerhalb der AISLER-Grenzen (0,125 mm; Via-Bohrung 0,25–0,45 mm, Restring 0,1 mm). Stand der AISLER-Seite: 07.07.2025. [Quelle]
- Bestückung: kleinste Bauform 0201, kleinstes Raster 0,35 mm für aktive Bauteile. Ob mit 0201 die Zollbezeichnung gemeint ist, steht nicht auf der Seite; bei AISLER nachfragen. [Quelle]
- AISLER setzt eigene Passermarken auf den Nutzenrand und nutzt keine Marken des Kunden. Eigene Passermarken brauchst du nur für andere Bestücker. [Quelle]
- Lötstoppstege zwischen den QFN-Pads: Bei 0,2 mm Lücke und 50 µm Maskenaufweitung bleiben 0,1 mm. Das ist genau der AISLER-Mindestwert; einzelne Stege können fehlen.
- Enddicke 1,6 mm ±10 %. Impedanzkontrolle mit Testcoupon gibt es nur auf Anfrage über ein Angebot. [Quelle]

**D5 – Prepreg-Dicke gegenüber der Nordic-Referenz (Information, im Vortest prüfen)**

Nordic verwendet 0,1 mm Prepreg zwischen Top und Lage 2. [Referenz] Im KiCad-Lagenaufbau für AISLER stehen 0,138 mm; die Recherche konnte den Wert nicht an der AISLER-Seite bestätigen, weil die Tabelle dort nur als Bild vorliegt. Nordic-Mitarbeiter empfehlen im Forum weniger als 100 µm und begründen das mit der Oberwellenabstrahlung. [Sekundär] Betroffen ist bei dir nur der kurze Abschnitt zwischen Pin 31 und dem Beginn der Aussparung. Ich halte das Risiko für überschaubar, es gehört aber in den Vortest (Oberwellen bei 4,9 und 7,3 GHz).

### E. Prüflabor, Kennzeichnung, Firmware

**E1 – Fertiger und Lagenaufbau festschreiben (hoch)**

`docs/fertigungsregeln.md` zielt darauf, das Board ohne Änderung bei AISLER, JLCPCB und Eurocircuits bestellen zu können. Für die DRC stimmt das. Für die Funkzulassung nicht: Prepreg-Dicke und Material unterscheiden sich (AISLER 0,138 mm, JLCPCB 7628 0,21 mm), und damit ändern sich Anpassung, Oberwellen und Antennenabstimmung. Geprüft wird ein konkretes Muster. Hersteller und Lagenaufbau gehören in die technische Dokumentation; bei einem Wechsel mindestens Antenne und Oberwellen neu messen.

**E2 – Kennzeichnung (mittel)**

Auf der Platine steht kein Text. [Layout] Die RED verlangt auf dem Gerät Typ-, Chargen- oder Seriennummer sowie Name und Anschrift des Herstellers; wenn die Größe das nicht zulässt, dürfen die Angaben auf die Verpackung oder in die Begleitunterlagen. Dazu kommen CE-Zeichen (mindestens 5 mm hoch), das Symbol der durchgestrichenen Mülltonne und der Knopfzellen-Warnhinweis. [unbestätigt, Details mit dem Labor klären]

Empfehlung für die Platine: Typ und Revision, Feld für Seriennummer oder Data-Matrix-Etikett, „+“ und „CR2032“ an den Batteriekontakten, Beschriftung der Testpunkte. Alles außerhalb des Antennenbereichs. AISLER druckt Text ab 0,8 mm Höhe und 0,15 mm Strichstärke.

**E3 – Vorbereitung für die Funkprüfung (Information)**

- Testfirmware: Direct Test Mode über TP8/TP9 oder Nordic „Radio Test“ mit Dauerträger und moduliertem Senden auf unterem, mittlerem und oberem Kanal sowie Empfangsbetrieb.
- Leitungsgebundene Messung: R2 entfernen, Koaxkabel an das `/RF`-Pad und die Masse-Lötfläche. Dem Labor ein vorbereitetes Muster mit Kabel mitgeben, zusätzlich unveränderte Muster für die Strahlungsmessung.
- Vortest der Oberwellen (2. und 3. Harmonische) lohnt sich; der Grenzwert für Nebenaussendungen oberhalb 1 GHz ist −30 dBm. [unbestätigt] Nordic gibt für das Referenzdesign mindestens 6 dB Abstand zu diesem Grenzwert an. [Sekundär]
- Nordic bietet eine kostenlose Layoutprüfung und Antennenabstimmung über ein privates DevZone-Ticket an. [Sekundär] Das würde ich nach Umsetzung von A1 und A2 nutzen.
- Ohne vorzertifiziertes Modul ist die vollständige Funk- und EMV-Prüfung des eigenen Designs nötig. [unbestätigt]

**E4 – Firmware und Dokumentation mit Bezug zur Zulassung (Information)**

- Seit 01.08.2025 gelten die Cybersicherheitsanforderungen der RED (Art. 3.3 d/e/f, Delegierte Verordnung 2022/30), harmonisierte Normen EN 18031-1/-2/-3 mit Einschränkungen. [Sekundär] Ob und welche Teile für dieses Gerät gelten, hängt von der Firmware ab (Pairing, personenbezogene Daten, Update-Mechanismus). Mit dem Labor klären. Hardwareseitig heißt das: Debug-Zugang in Seriengeräten sperren (APPROTECT), Updates signieren.
- Hinweis im Schaltplan beachten: Der STCC4 braucht mindestens 2,7 V, die CR2032 fällt bei Sendepulsen darunter. Für die Störfestigkeitsprüfung muss das Gerät in einem definierten Betriebszustand laufen.

## 5. Rechtlicher Rahmen in Kürze

Diese Tabelle ist eine Orientierung, keine Rechtsauskunft. Versionsstände vor der Konformitätserklärung an der aktuellen Liste im EU-Amtsblatt prüfen.

| Regelwerk | Inhalt | Stand der Prüfung |
|---|---|---|
| RED 2014/53/EU | Sicherheit (3.1a), EMV (3.1b), Funkspektrum (3.2) | [unbestätigt] |
| EN 300 328 V2.2.2 | Funkparameter 2,4 GHz, harmonisiert seit 2020 | [Sekundär], aktuelle Liste 2026 nicht geprüft |
| EN 301 489-17 V3.3.1 | EMV für Breitband-Datenübertragung, gelistet im Durchführungsbeschluss (EU) 2025/893 mit Einschränkung | [Quelle] |
| EN 301 489-1 | EMV-Grundnorm, im Beschluss 2025/893 nicht gelistet | [Quelle] |
| EN 62368-1 | Produktsicherheit, Knopfzellen | [unbestätigt] |
| EN 62479 | HF-Exposition, Ausnahme für Geräte unter 20 mW | [unbestätigt] |
| RED Art. 3.3 d/e/f, EN 18031 | Cybersicherheit, anzuwenden seit 01.08.2025 | [Sekundär] |
| Batterieverordnung (EU) 2023/1542 | Art. 11 Austauschbarkeit ab 18.02.2027, Kennzeichnung | [Quelle] für Art. 11 |
| RoHS 2011/65/EU, REACH, WEEE/ElektroG | Stoffbeschränkungen, Registrierung bei der stiftung ear | [unbestätigt] |
| Spielzeugrichtlinie 2009/48/EG | gilt nur, wenn das Gerät als Spielzeug für Kinder unter 14 bestimmt ist; ein Lehrmittel für Schule und Hochschule fällt vermutlich nicht darunter | [unbestätigt], bei der Zweckbestimmung festlegen |
| Cyber Resilience Act (EU) 2024/2847 | Meldepflichten ab 11.09.2026, Hauptpflichten ab 11.12.2027 | [Sekundär] |

Ein Beschluss (EU) 2026/2003 vom September 2026 ändert die Liste der harmonisierten Normen; sein Inhalt konnte in der Recherche nicht gelesen werden.

## 6. Offene Fragen

1. Wie wird die Knopfzelle gehalten und kontaktiert (Clip auf TP11, Feder, Halter im Gehäuse)? Davon hängt C1 ab.
2. Wozu dient das 3,2-mm-Loch: Trageband oder Gehäuseschraube?
3. Gibt es schon ein Gehäusekonzept (Material, Öffnungen für die Sensoren, Lage der Fugen)?
4. Welche Sendeleistung und welche BLE-Betriebsarten (1M, 2M, Coded) sind geplant?
5. Welche Chiprevision des nRF54L15 wird verbaut (wegen der Errata)?

## 7. Quellen

Nordic
- Referenzlayout nRF54L15-QFAA v0.8 (Schaltplan, Stückliste, Lagen-PDF, Altium-Dateien): https://www.nordicsemi.com/Products/nRF54L15/Reference-layout
- DevZone, C5 = 2,2 nF: https://devzone.nordicsemi.com/f/nordic-q-a/127508/issue-with-capacitor-c5-value-in-the-54l15-qfaa-qfn-48-reference-circuit
- DevZone, Prepreg-Dicke und Oberwellen: https://devzone.nordicsemi.com/f/nordic-q-a/126808/nrf54l15-design-verification/561005
- DevZone, Anpassnetzwerk und Lagenaufbau: https://devzone.nordicsemi.com/f/nordic-q-a/128369/regarding-matching-network-nrf54l15/567554
- Errata nRF54L15: https://docs.nordicsemi.com/r/bundle/errata_nrf54l15_rev2/page/err/nrf54l15/rev2/latest/err_l15.html

Antenne
- TI AN043 (SWRA117D), Small Size 2.4 GHz PCB antenna: https://www.ti.com/lit/an/swra117d/swra117d.pdf
- Infineon AN91445, Antenna Design and RF Layout Guidelines: https://www.infineon.com/dgdl/Infineon-AN91445_Antenna_Design_and_RF_Layout_Guidelines-ApplicationNotes-v09_00-EN.pdf

AISLER
- Design-Regeln 4 Lagen ENIG (Stand 07.07.2025): https://community.aisler.net/t/4-layer-1-6mm-enig-design-rules/3733
- Lagenaufbau 4 Lagen 1,6 mm 35 µm mit Impedanztabelle: https://community.aisler.net/t/4-layers-1-6mm-35-m-stackup/5457
- Impedanzprüfung und koplanare Leitungen: https://community.aisler.net/t/coplaner-waveguide-impedance-testing/5456
- Bestückung: https://community.aisler.net/t/assembly-portfolio/4674
- Nutzenspezifikation (2023): https://community.aisler.net/t/aisler-production-panel-specification/2779

Regulierung
- Durchführungsbeschluss (EU) 2025/893: https://eur-lex.europa.eu/eli/dec_impl/2025/893/oj
- Durchführungsbeschluss (EU) 2020/167 (EN 300 328 V2.2.2): https://eur-lex.europa.eu/eli/dec_impl/2020/167/oj/eng
- Durchführungsbeschluss (EU) 2025/138 (EN 18031): https://eur-lex.europa.eu/eli/dec_impl/2025/138/oj/eng
- Leitlinien der Kommission zu Art. 11 Batterieverordnung, C/2025/214: https://eur-lex.europa.eu/eli/C/2025/214/oj/eng

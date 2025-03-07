# KiCad-Blockwoche

## Installation

Unter Debian/Ubuntu kann KiCad mit den folgenden Befehlen installiert werden:

```sh
sudo apt install -t bookworm-backports kicad kicad-symbols kicad-footprints kicad-packages3d
```

---

## Tag 1

### Erste Schritte & Einstellungen

- Beim ersten Start die **globale Bibliotheksauswahl** auf Standard belassen.

### Steuerung

#### **Schaltplan-Editor**

- `A` – Bauteil hinzufügen
- `R` – Bauteil rotieren
- `X` – Bauteil spiegeln
- `W` – Draht/Wire zeichnen
- `E` – Bauteil bearbeiten
- `V` – Wert eines Bauteils ändern
- `G` – Bauteil verschieben (ohne Drähte zu trennen)
- `M` – Bauteil verschieben (mit Drähten)
- `Del` – Bauteil oder Draht löschen
- `Ctrl + Z` – Rückgängig machen

#### **PCB-Designer**

- `X` – Leiterbahnen (Traces) zeichnen
- `M` – Bauteil verschieben
- `R` – Bauteil rotieren
- `Ctrl + M` – Bauteil mit Relativbewegung verschieben
- `Ctrl + Z` – Rückgängig machen
- `Ctrl + Y` – Wiederholen
- `Del` – Element löschen

### Workflow

1. **Schaltplan zeichnen**
2. **Elektrische Regelprüfung (ERC) durchführen**
3. **Bauteile im Layout-Editor platzieren**
4. **Leiterbahnen routen** (komponenten verbinden)

### Wichtige Learnings

- Nach der **Zuweisung von Footprints**:
    - **Bauteile kopieren & Werte ändern, ohne die Teilenummern anzupassen, führt zu Problemen** bei der Fertigung.
- **Alle Power-Labels benötigen eine Power-Flag** (`pwr_flag`), damit die elektrische Regelprüfung keine Fehler meldet.

---

## Tag 2

### Arbeiten mit Symbolen

- Symbole können über den **Symbol-Importer** importiert werden.
- Nach dem Import sollte die Zuweisung von Symbolen zu den jeweiligen Footprints überprüft werden.

mit dem druekcen von e mit unselektiertem curses lassen sich die values aendern. und informationen wie 
datenblaetter und bestell informationen hinterlegen

### Shortcuts
p place pin  
e edit properties  

### Zusätzliche Tipps

- **Bibliotheksverwaltung:**
  - Falls benötigte Symbole oder Footprints fehlen, können eigene Bibliotheken angelegt werden.
  - Externe **KiCad-Bibliotheken** können über `Preferences → Manage Symbol Libraries` hinzugefügt werden.

### Datenbank

#### Treiber herunterladen und Installieren

```sh
sudo apt-get install iodbc
```

[https://www.unixodbc.org/](https://www.unixodbc.org/)

```sh
./configure
make
make install
```

[http://www.ch-werner.de/sqliteodbc/](http://www.ch-werner.de/sqliteodbc/)  
[http://www.ch-werner.de/sqliteodbc/html/index.html](http://www.ch-werner.de/sqliteodbc/html/index.html)

```sh
./configure && make
make install
```

#### Treiber Registrieren

```sh
sudo odbcinst -i -s -f /etc/odbc.ini
sudo odbcinst -i -d -f /etc/odbcinst.ini
```

Datenbank in KiCad hinzufügen. Wenn die Treiber korrekt installiert wurden, sollte keine Fehlermeldung auftauchen und die Komponenten tauchen mit dem gegebenen Namen in der Part-Selektion auf.

Neue Komponenten müssen dann über ein Datenbank-Tool in die Datenbank geschrieben werden. Die Symbole werden in KiCad erstellt und dann in der Datenbank verknüpft.

**Tabellenstruktur Datenbank**

| Feldname         | Beschreibung                                                    |
|-----------------|----------------------------------------------------------------|
| PartID          | Eindeutige ID des Bauteils in der Datenbank                    |
| Symbol         | Referenz auf das Schaltplansymbol                               |
| Footprint       | Zugehöriges Footprint-Modell für die Platine                   |
| Value           | Bauteilwert (z. B. Widerstandswert, Kapazität)                 |
| Designator      | Bezeichnung des Bauteils im Schaltplan (z. B. R1, C3, U5)      |
| Category        | Bauteilkategorie (z. B. Widerstand, Kondensator, IC)           |
| MFR             | Hersteller (Manufacturer) des Bauteils                         |
| MPN             | Herstellernummer (Manufacturer Part Number)                    |
| Rating          | Elektrische Spezifikationen des Bauteils (z. B. Spannung, Strom) |
| Remark         | Zusätzliche Anmerkungen oder Hinweise                          |
| Datasheet       | Link oder Pfad zum Datenblatt des Bauteils                     |
| Dist1           | Distributor 1 (Händler für das Bauteil)                        |
| Dist1PN         | Teilenummer bei Distributor 1                                  |
| Dist2           | Distributor 2                                                 |
| Dist2PN         | Teilenummer bei Distributor 2                                 |
| Dist3           | Distributor 3                                                 |
| Dist3PN         | Teilenummer bei Distributor 3                                 |
| Active          | Gibt an, ob das Bauteil aktiv oder abgekündigt ist             |
| Description     | Detaillierte Beschreibung des Bauteils                         |
| FootprintFilters | Filter für alternative Footprints                             |
| Keywords        | Stichworte für die Suche nach dem Bauteil                      |
| NoBom           | Gibt an, ob das Bauteil in die Stückliste (BOM) aufgenommen wird |
| SchematicOnly   | Bauteil existiert nur im Schaltplan, nicht auf der Platine     |
| Version         | Versionsnummer des Bauteils in der Datenbank                   |
| Created        | Datum der Erstellung des Bauteileintrags                        |
| Modified       | Datum der letzten Änderung des Bauteileintrags                  |

---

## Tag 3

### Arbeiten mit großen Schaltplänen

#### Schaltpläne über mehrere Seiten aufteilen
- Ermöglicht eine bessere **Strukturierung** und **Übersichtlichkeit**.
- Besonders hilfreich für **komplexe Schaltungen** mit vielen Komponenten.

#### Verwendung von Labels für Verbindungen
- Durch die Nutzung **gleichnamiger Labels** können Signale und Netze zwischen verschiedenen Seiten eines Schaltplans verbunden werden.
- Dies **ersetzt lange Drahtverbindungen** und sorgt für eine **saubere Darstellung**.  
  **Globale und lokale Labels beachten.**

#### Hierarchische Schaltpläne verwenden
- **Module** können in **separaten Hierarchieblöcken** organisiert werden.
- Erleichtert das **Wiederverwenden** von Schaltungsblöcken.
- **Verbessert die Lesbarkeit** des Schaltplans.

#### Netzklassen definieren
- **Netzklassen** helfen, verschiedene Signalarten (z. B. **Stromversorgung, Signalleitungen, Hochfrequenzleitungen**) zu organisieren.
- Dies erleichtert das **Einhalten von Design-Richtlinien** und **DRC-Prüfungen**. Diese können dann beim Layout eingestellt werden.

**(Zuweisungen ergänzen)**

#### BUS erstellen
- **Dicke blaue Leitung** zeichnen.
- **Label hinzufügen** und Bus-Elemente angeben (Example: `B0 - 13, CLK, IO`). Label name B[0..15]
- **Rechtsklick auf Bus → Unfold** wählen und dann die einzelnen Pins verbinden.
- **Bus-Aliase** können in den **Blatteigenschaften** neben dem Speichersymbol hinzugefügt werden.

#### Electrical Rule Check (ERC) Einstellungen
- **Input zu Input gibt keinen Fehler**. Diese sind einstellbar.

#### Capacity Bias Effect
- **Kapazitive Effekte durch Layout beachten.**
erklaeren was es macht.. hoehere spannung kleine kapazitaet

#### Pin Helper
- **Rechtsklick auf einen Pin → Pin Helper** wählen.
- **Label und Pin** direkt zuweisen.

### Footprints

- **3D-Modelle hinzufügen**, um Footprints visuell zu überprüfen.
- **Abmessungen aus Datenblättern entnehmen**.
- **Pads erstellen** mit dem Kreis-Icon.
- **Doppelklick auf ein Pad**, um **Größe und Position** festzulegen.
- **Eingabefelder unterstützen Berechnungen** (z. B. `5mm + 2mm`).

#### Zusätzliche Ebenen
- **Fab-Layer** für Fertigungsinformationen.
- **Silkscreen-Layer** für Beschriftungen auf der Platine.
- **Margin-Layer** zur Definition von Platinenrändern.


## Tag 4

### Vias
Bei Vias müssen einige Dinge beachtet werden, unter anderem:
- **Lagenaufbau und Möglichkeiten** → Vorab den **Produzenten** wählen und abklären.
- **Auswirkungen** → Kosten & Produktionszeit berücksichtigen.
- **Minimale Lochdurchmesser** → Typisch **0.3 mm**.
- **Restring** → Typisch **0.15 mm**.
- **Keine Vias im Pad** → Lötzinn fließt weg, außer verschlossen (→ teuer!).

---

### Einstellungen
**Pfad:** *File → Board-Setup*

#### **Physical Stackup**
- Anzahl **Layer** und deren **Eigenschaften/Konfiguration** festlegen.

#### **Constraints**
- Design-Regeln und **Mindestabstände** definieren.

#### **Netzklassen**
- **Gruppierung von Signalen** (z. B. **Power, GND, Signal**).

#### **Custom Rules**
- **Eigene Design-Regeln** für spezifische Anforderungen erstellen.

---

### PCB Designer

#### **Bauteile importieren**
- Fehlermeldungen beachten → zeigen **fehlende Footprints** oder andere **Komplikationen**.

#### **Schritt 1: Board zeichnen**
- **Edge Cuts** Layer für die **Platinenkontur** nutzen.

#### **Schritt 2: Bauteile platzieren**
- **Grob platzieren**, dann **Feinjustierung**.
- **Zentrale Komponenten zentrieren** (z. B. Mikrocontroller).
- **Ausrichtung beachten** → USB-Pins zum **USB-Stecker**.
- **Abstände einhalten**:
  - **Digitale und analoge Bereiche trennen**.
  - **Kurze Verbindungen für empfindliche Signale**.
  - **Kondensatoren möglichst nah an ICs platzieren**.

---

### **Tipps**
- **Nur Footprints selektierbar machen**, um **versehentliches Verschieben** von Verbindungen zu vermeiden.

Einige Bauteile wie Spannungsregulatoren oder Oszillatoren geben gewisse Anordnungen der Bauteile vor. Diese gemaess Datenblatt anordnen und verbinden.

---

### **Frage**
> Ist es möglich, nachträglich einen **Footprint** hinzuzufügen, sodass er automatisch mit bereits im **Schaltplan vorhandenen Bauteilen** synchronisiert wird?

## Tag 5

### **Flächen & Polygone (Filled Zones)**
- **Bereich zeichnen**, um eine gefüllte Kupferfläche zu erstellen.
- **Verbindung wählen** → Typischerweise **Speisung (VCC) oder Ground (GND)**.
- **Mit `B` neu generieren**, um die Zone zu aktualisieren.

#### **Eigenschaften der Zone anpassen**
1. **Zone anklicken** und **Eigenschaften öffnen**.
2. Unter **Pad Connection** den **Thermal Relief** Modus wählen.

---

### **Differentielles Layout für USB**
#### **Schritt 1: Netzlabels im Schematic Setup verwalten**
- **USB-Signale** in der **Netclass-Verwaltung** zuweisen.

#### **Schritt 2: Differenzpaar im Layout erstellen**
1. **`6` auf das Pad klicken**.
2. Zwei **parallele Leitungen** sollten automatisch erstellt werden.

---

## **Bill of Material (BOM)**
Die **Stückliste (Bill of Material, BOM)** enthält alle benötigten Bauteile für die Fertigung der Platine. Sie stellt sicher, dass alle Bauteile korrekt bestellt und bestückt werden.

### **Typische Informationen in der BOM**
- **Bauteilbezeichnung** (Designator, z. B. R1, C3, U5)
- **Bauteilwert** (z. B. 10kΩ, 100nF)
- **Footprint** (z. B. SMD0603, THT)
- **Hersteller & Bestellnummer** (MFR, MPN)
- **Distributor & Bestellnummer** (z. B. Mouser, Digikey, LCSC)
- **Anzahl der Bauteile** für die Fertigung

### **Export der BOM in KiCad**
Die BOM kann über **File → Fabrication Outputs → BOM** exportiert werden.  
Es gibt verschiedene **BOM-Plugins**, die z. B. eine CSV oder Excel-Datei generieren.

---

## **Gerber-Dateien**
**Gerber-Dateien** sind das standardisierte Format für die Herstellung von Leiterplatten. Sie enthalten die **grafische Darstellung** der verschiedenen PCB-Layer, die von Herstellern genutzt werden.

### **Typische Gerber-Dateien**
- **Leiterbahnen & Kupferflächen** (z. B. `Top_Copper`, `Bottom_Copper`)
- **Lötstopplack & Bestückungsdruck** (Solder Mask & Silkscreen)
- **Platinenumrisse** (Edge Cuts)

### **Gerber-Export in KiCad**
1. **File → Fabrication Outputs → Gerber**
2. **Alle relevanten Layer auswählen** (Top, Bottom, Silkscreen, Soldermask, etc.).
3. **Gerber-Dateien exportieren und in einem Viewer kontrollieren**.

---

## **Bohrdaten (Drill Files)**
Die **Bohrdaten** definieren alle Löcher, die auf der Platine gebohrt werden müssen. Sie sind besonders wichtig für bedrahtete Bauteile, Vias und Montagelöcher.

### **Typische Inhalte der Bohrdaten**
- **THT-Bohrungen** für bedrahtete Bauteile.
- **Vias** für Lagenverbindungen zwischen PCB-Schichten.
- **Montagelöcher** für Gehäuse oder Befestigungen.

### **Bohrdaten-Export in KiCad**
1. **File → Fabrication Outputs → Drill Files**
2. **Bohrdurchmesser & -typen überprüfen**.
3. **Bohrdateien zusammen mit den Gerber-Dateien an den Hersteller senden**.

---

## **Pick & Place Daten**
Die **Pick & Place Datei** enthält die **Positionen der Bauteile** für die automatische Bestückung mit Maschinen. SMT-Bauteile werden anhand dieser Datei automatisch auf die Platine gesetzt.

### **Typische Inhalte der Pick & Place Datei**
- **X- & Y-Koordinaten** der Bauteile.
- **Drehwinkel der Bauteile**.
- **Footprint & Referenzdesignator** (z. B. `R1`, `C3`).

### **Pick & Place-Export in KiCad**
1. **File → Fabrication Outputs → Footprint Position File**
2. **Format auswählen (CSV, TXT, etc.)**.
3. **Datei an den Bestücker senden**.

---

# KiCad-Blockwoche

## Installation

Unter Debian/Ubuntu kann KiCad mit den folgenden Befehlen installiert werden:

```
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

install odbc driver

Treiber herunterladen

```sh
sudo apt-get install iodbc
```

https://www.unixodbc.org/

```sh
./configure
make
make install
```

http://www.ch-werner.de/sqliteodbc/
http://www.ch-werner.de/sqliteodbc/html/index.html


```sh
./configure && make
make install
```


Treiber Registrieren
```
sudo odbcinst -i -s -f /etc/odbc.ini
sudo odbcinst -i -d -f /etc/odbcinst.ini
```
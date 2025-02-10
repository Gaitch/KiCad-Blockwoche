# KiCad-Blockwoche

## Installation

```sh
sudo apt install -t bookworm-backports kicad kicad-symbols kicad-footprints kicad-packages3d
```

## Day1

### Settings
use the default global library selection

### Controls

---- Schematic ----

a add component

r rotate component

x mirror component

w draw wire

e edit component 

v change value

---- PCB Designer ----

x draw traces

### Workflow
1. Draw Schematic
2. use electrical rule check
3. place components in Designer
4. start moving and connect traces

### Learnings

after assign footprints

copy pasting components and adjusting the value without changing the 
partnumbers will lead to errors in assembly.

all power labels need a power flag (pwr_flag)
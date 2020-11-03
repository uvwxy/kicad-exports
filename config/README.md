# sample configurations

## bom.kibot.yaml

does generate default Bill of Material list
output:
 - docs/bom/%f.%

## octopart/bom.kibot.yaml
does generate Bill of Material which fits the needs of [octopart](https://octopart.com/)

output:
 - gerbers/%f_octopart.csv

## docs.kibot.yaml
does generate schematic and board designs in pdf and svg

output:
 - docs/%f_schematic.pdf
 - docs/%f_schematic.svg
 - docs/%f_board.pdf

## gerbers.kibot.yaml
does generate gerber and drill files

output:
 - gerbers/%f_*.gbr
 - gerbers/%f_*.drl

## model.kibot.yaml
does generate 3D CAD file

output:
 - cad/%f.step

## plot.kibot.yaml
does generate render of front and bottom PCB

output:
 - docs/%f_board_top.svg
 - docs/%f_board_bottom.svg

## oshpark/plot_afterdark.kibot.yaml 
does generate render of front and bottom PCB

## position.kibot.yaml
does generate default position file used by pick and place machienes

output:
 - gerbers/%f_position.csv

## position_jlcpcb.kibot.yaml
does generate position file which fits the needs of [JLCPCB](https://jlcpcb.com/)

output:
 - gerbers/%f_position_jlcpcb.csv

## report.kibot.yaml

---

%f original pcb/sch file name without extension.
%p pcb/sch title from pcb metadata.
%c company from pcb/sch metadata.
%r revision from pcb/sch metadata.
%d pcb/sch date from metadata if available, file modification date otherwise.
%D date the script was started.
%T time the script was started.
%i a contextual ID, depends on the output type.
%x a suitable extension for the output type.

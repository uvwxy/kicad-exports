[![CodeFactor](https://www.codefactor.io/repository/github/nerdyscout/kicad-exports/badge)](https://www.codefactor.io/repository/github/nerdyscout/kicad-exports) <a href="https://discord.gg/j4yEeDngbw"><img src="https://img.icons8.com/ios/72/discord-logo.png" width=20></a>

Auto generate exports (schematics, gerbers, plots) for any KiCAD5 project.
You could run it locally or on every `git push` with Github Actions.

# usage of kicad-exports with Github Actions
```yaml
name: example

on:
  push:
    paths:
    - '**.sch'
    - '**.kicad_pcb'
  pull_request:
    paths:
      - '**.sch'
      - '**.kicad_pcb'

jobs:
  example:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v2
    - uses: nerdyscout/kicad-exports@v2.1
      with:
      # Required - kibot config file
        config: docs.kibot.yaml
      # optional - prefix to output defined in config
        dir: docs
      # optional - schematic file
        schema: '*.sch'
      # optional - PCB design file
        board: '*.kicad_pcb'
    - name: upload results
      uses: actions/upload-artifact@v2
      with:
        name: docs
        path: docs
```
The [predefined configs](/config) do run a ERC and DRC in advance, if these checks fail no exports will be generated. You could [write your own config](https://github.com/INTI-CMNB/kibot/tree/v0.7.0#the-configuration-file) file and define [filters](https://github.com/INTI-CMNB/kibot#filtering-drcerc-errors) to ignore these errors therefore forcing to export the data. In this case be careful not to end up with some faulty PCB.

## predefined configs
 - [bom.kibot.yaml](config/bom.kibot.yaml) generates [interactive BoM](https://github.com/openscopeproject/InteractiveHtmlBom), [kibom.xlsx](https://github.com/SchrodingersGat/KiBoM#xlsx-output) and [kibom.csv](https://github.com/SchrodingersGat/KiBoM#csv-output) in folder `docs/bom/`
 - [docs.kibot.yaml](config/docs.kibot.yaml) plots the PCB with user layers and schematics as *.pdf and *.svg in folder `docs/` 
 - [fabrications.kibot.yaml](config/fabrications.kibot.yaml) generates all gerber data, [bom.csv](https://github.com/SchrodingersGat/KiBoM#csv-output) with all components and position.csv, used for Pick and Place maschienes, in folder `gerbers/`
 - [model.kibot.yaml](config/model.kibot.yaml) does generate 3D step file in folder `cad/`
 - [plot.kibot.yaml](config/plot.kibot.yaml) runs [pcbdraw](https://github.com/yaqwsx/PcbDraw) to generate plots of bottom and top of the PCB into `docs/img/`
 - [report.kibot.yaml](config/report.kibot.yaml) just runs DRC and ERC

# use kicad-exports local 
## Installation
You need to have [Docker](https://www.docker.com/) installed.

```
git clone --recursive https://github.com/nerdyscout/kicad-exports /some/where/kicad-exports
cd /some/where/kicad-exports
make && make install
```

## run
go to your KiCad project folder and run kicad-exports
```
cd /my/kicad/example-project
kicad-exports -d $DIR_OUT -e $SCHEMA -b $BOARD -c $CONFIG 
```
:warning: running any command your git repository will be modified using [kicad-git-filters](https://github.com/INTI-CMNB/kicad-git-filters/tree/v1.0.1).

### run with predefined example config
```
kicad-exports -c docs.kibot.yaml 
```
### run with own config
place config file in directory of your kicad project and use relative path.
```
kicad-exports -c myconfig.kibot.yaml -v -s all
```
running localy enables additional paramaters
- `-v, --verbose` is useful while developing own config files
- `-s, --skip $arg` skips preflight from given config file 

# Credits
- [KiBot](https://github.com/INTI-CMNB/kibot)
- [KiBoM](https://github.com/SchrodingersGat/KiBoM)
- [IBoM](https://github.com/openscopeproject/InteractiveHtmlBom/wiki/Usage)
- [kicad-automation-scripts](https://github.com/INTI-CMNB/kicad-automation-scripts)
- [PcbDraw](https://github.com/yaqwsx/PcbDraw)
- [kicad-git-filters](https://github.com/INTI-CMNB/kicad-git-filters)

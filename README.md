# Table of Contents
1. [Day 1:](#day1)
    - [Get Familiar with Open-Source EDA Tools](#get-familiar-with-open-source-eda-tools)
    - [Intro to OpenLane](#intro-to-openlane)
    - [Directory Structure of Openlane:](#directory-structure-of-openlane)
    - [Steps to characterize synthesis results](#steps-to-characterize-synthesis-results)
   
2. [Day 2:](#day2)
    - [Steps to run floorplan using OpenLANE](#steps-to-run-floorplan-using-openlane)
    - [Review floorplan layout in Magic](#review-floorplan-layout-in-magic)
    - [Congestion aware placement using RePlAce](#congestion-aware-placement)
---
# Day1:<a name ="day1"></a>
## Get Familiar with Open-Source EDA Tools<a name="get-familiar-with-open-source-eda-tools"></a>

### Basic Bash Commands:
- `cd`   : Change directory.
- `ls`   : List all files.
- `mkdir`: Make a directory.
- `pwd`  : Show the current directory.
- `clear`: Clear the terminal.
- `tree` :This command is used to print the hierarchy of the file system from the present directory.

### Intro to OpenLane:<a name="intro-to-openlane"></a>
- OpenLane is an automated RTLtoGDSII flow. It consists of a variety of open-source tools including:
<div align="center">

|      Software           |            Description                          |
|:-----------------------:|:-----------------------------------------------:|
|         Yosys           |            Verilog synthesis tool               |
|         Magic           |                Layout editor                    |
|         Netgen          |        Digital netlist comparison tool          |
|         Fault           |            Digital fault simulator              |
| CVC SPEF-Extractor      |  Circuit verification and analysis tool         |
|          CU-GR          |              Global routing tool                |
|         Klayout         |      Mask layout editor and viewer              |

</div>


## Directory Structure of Openlane:<a name="directory-structure-of-openlane"></a>
- This the directory structure of the openlane

```

openlane
├── AUTHORS.md
├── clean_runs.tcl
├── configuration
├── conf.py
├── CONTRIBUTING.md
├── default.cvcrc
├── designs
├── docker_build
├── docs
├── flow.tcl
├── LICENSE
├── Makefile
├── README.md
├── regression_results
├── report_generation_wrapper.py
├── run_designs.py
└── scripts
```
- In the designs folder we have a couple of designs that comes witht the openlane.Thefollowing designs are included in the openlane.
```

designs
├── 151
├── aes
├── aes128
├── aes192
├── aes256
├── aes_cipher
├── aes_core
├── APU
├── blabla
├── BM64
├── chacha
├── cic_decimator
├── des
├── des3
├── digital_pll_sky130_fd_sc_hd
├── genericfir
├── inverter
├── jpeg_encoder
├── ldpc_decoder_802_3an
├── ldpcenc
├── manual_macro_placement_test
├── md5
├── ocs_blitter
├── picorv32a
├── point_add
├── point_scalar_mult
├── PPU
├── README.md
├── s44
├── salsa20
├── sha3
├── sha512
├── sound
├── spm
├── synth_ram
├── usb
├── usb_cdc_core
├── usbf_device
├── wbqspiflash
├── xtea
├── y_dct
├── y_huff
└── zipdiv
```
-  For Demonstration of tools, we have choosen the picorv32a project.Below are the contents of the picrorv32a.
```
picorv32a
.
├── config.tcl
├── runs
│   ├── 10-04_20-22
│   ├── 11-04_07-34
│   ├── 12-04_08-02
│   └── 12-04_14-17
├── sky130A_sky130_fd_sc_hd_config.tcl
├── sky130A_sky130_fd_sc_hdll_config.tcl
├── sky130A_sky130_fd_sc_hs_config.tcl
├── sky130A_sky130_fd_sc_ls_config.tcl
├── sky130A_sky130_fd_sc_ms_config.tcl
└── src
    ├── picorv32a.sdc
    └── picorv32a.v
```
- This config.tlc file contains every details about the design. for example, details about enrollment, clock period, clock period port etc.The setting in the config.tcl has higher precedence compared to the default canfigurations. These settings will override the default settings of the openlane.

## Design Preparation Step:
- To access the openlane tool first we have to change the directory to the 
`/<path to openlane_working_dir>/openlane`, we can access the files .
In this folder we have to type
```
$  docker
$ ./flow.tcl --interactive
```
- Here we are using `--interactive` , because we are manually running every tool by overselves.If we just execute `./flow.tcl` The entire process from RTL to GDS will we completed.

![image](https://i.imgur.com/OzGRR6A.jpeg)

- we need add the required  packages , the below command does that.
```
$package require openlane 0.9
```
- before running the synthesis , we have to prep the design. 
```
$ prep -design picorv32a
```

![image](https://i.imgur.com/00Mqqo7.png)

- Result for the above command.

![image](https://i.imgur.com/D3yTMWZ.png)
- Preparation state  is done ,Now we can processed with the synthesis.Now we executing the  command.
  ```
    $ run_synthesis
  ```

![image](https://i.imgur.com/GrLreJt.png)

- After synthesis , If every thing goes correctly, we can see the terminal result as,

![image](https://i.imgur.com/9dYgzPi.png)

## Steps to characterize synthesis results:<a name="steps-to-characterize-synthesis-results"><\a>
-From the sysnthesis results, we can observe a report printed on the console.

![image](https://i.imgur.com/ZoNDdPU.png)

```
flop ratio = (number of flip flops) / (number of total cells)
```
- From the above data we can absorve that,flip flop ratio is 10.84%.
- After completing the synthesis process, we can now confirm that all the mappings have been performed by ABC.

![image](https://i.imgur.com/oQIMLsn.png)
-In the report, we can identify the completion time of the actual synthesis. The synthesis statistics report displayed below is consistent with the previously observed information.

## Day2: <a name="day2"></a>
# Steps to run floorplan using OpenLANE:<a name="steps-to-run-floorplan-using-openlane"></a>
- To create a floorplanning, we need to execute the command.
```
$ run_floorplan
```

![image](https://i.imgur.com/SD6Bs9e.png)

- If floorplan execute without any error's we should get the terminal output like this.

![image](https://i.imgur.com/mwCxfzn.png)
- Floorplan results are created in the below locations.
```
vsduser@vsdsquadron:~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/14-04_05-57/results/floorplan$ ls
merged_unpadded.lef  picorv32a.floorplan.def  picorv32a.floorplan.def.png
```
- picorv32a.floorplan.def contains the floorplaning information and picorv32a.floorplan.def.png is a image of teh floorplan., you can see the image below.

![picorv32a.floorplan.def.png](https://i.imgur.com/QKvhOp2.png)

# Review floorplan layout in Magic : <a name="review-floorplan-layout-in-magic"></a>
- To view  and edit the floor plan we ca use magic, we need to execute the command
```
magic -T /home/<user name>/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.floorplan.def
```

![magic command](https://i.imgur.com/7H2BDcB.png)
- After executing the above command you will find a pop window of magic which should look like,

![magic window](https://i.imgur.com/D7yu9ft.png)
- Then point eh mouse on the design , and press `s`, which will select the design and then press `v` which will make the design center.

![magic s+v](https://i.imgur.com/H6kHeal.png)
- on the design press left click on a corner and right click on other place to select the a portion of the design, then we have to press `z` to zoom that section. In magic we have to press `s` to select any sections, so we randomly choose a component and press 's' ,and shift magic command window and execute 
```
% what
```
- This will show the deatils of the selected component.

![image](https://i.imgur.com/GHUNyvY.png)
- you can observer the the Decap cells are arranged at the border of the side rows.

![image](https://i.imgur.com/miUYCnj.png)
# Congestion aware placement using RePlAce<a name ="congestion-aware-placement"></a>
- After floorplanning , we should go to do placement. As we know we have two type of placements.Global placement and deatiled placement, When we run the placement, first Global placement is happens. main objective of glibal placement is to reducing the length of wires.
- we make the placement by executing the command
```
run_placement
```
- The results of this command are stored the in placement folder in the results.The below shows the results,
```
vsduser@vsdsquadron:~/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/14-04_05-57/results/placement$ l -ltr
total 3892
lrwxrwxrwx 1 vsduser vsduser      29 Apr 14 11:27 merged_unpadded.lef -> ../../tmp/merged_unpadded.lef
-rw-r--r-- 1 vsduser vsduser 3164924 Apr 14 12:14 picorv32a.placement.def
-rw-r--r-- 1 vsduser vsduser  816328 Apr 14 12:14 picorv32a.placement.def.png
```
- The image `picorv32a.placement.def.png` is the snapshot of the placemnt that is automatilcally created.

![image](https://i.imgur.com/Ge9q8iE.png)

- we view the plcament in magic, execute the command, from the `/Desktop/work/tools/openlane_working_dir/openlane/designs/picorv32a/runs/14-04_05-57/results/placement` folder.
```
magic -T /home/vsduser/Desktop/work/tools/openlane_working_dir/pdks/sky130A/libs.tech/magic/sky130A.tech lef read ../../tmp/merged.lef def read picorv32a.placement.def
```
- After executing the command you the magic window will pop and up and you can observe the floorplan.In zoom image you can see the global palcement.

![image](https://i.imgur.com/0nc8OdA.png)
![image](https://i.imgur.com/CCjnruN.png)

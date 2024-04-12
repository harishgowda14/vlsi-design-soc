# Table of Contents
1. [Day 1:](#day1)
    - [Get Familiar with Open-Source EDA Tools](#get-familiar-with-open-source-eda-tools)
    - [Intro to OpenLane](#intro-to-openlane)
    - [Directory Structure of Openlane:](#directory-structure-of-openlane)
    - [Steps to characterize synthesis results](#steps-to-characterize-synthesis-results)
---

## Get Familiar with Open-Source EDA Tools<a name="get-familiar-with-open-source-eda-tools"><\a>

### Basic Bash Commands:
- `cd`   : Change directory.
- `ls`   : List all files.
- `mkdir`: Make a directory.
- `pwd`  : Show the current directory.
- `clear`: Clear the terminal.
- `tree` :This command is used to print the hierarchy of the file system from the present directory.

### Intro to OpenLane<a name="intro-to-openlane"></a>
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


## Directory Structure of Openlane:<a name="directory-structure-of-openlane"></a>- This the directory structure of the openlane

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
$ run_synthesis
```
- ![image](https://i.imgur.com/00Mqqo7.png)
- Result for the above command.
![image](https://i.imgur.com/D3yTMWZ.png)
- Preparation state  is done ,Now we can processed with the synthesis.Now we execute  `$ run_synthesis`.
![image](https://i.imgur.com/GrLreJt.png)
- After synthesis , If every thing goes correctly, we can see the terminal result as,
- ![image](https://i.imgur.com/9dYgzPi.png)

## [Steps to characterize synthesis results](<a name="steps-to-characterize-synthesis-results"><\a>
-From the sysnthesis results, we can observe a report printed on the console.Like below one
![image](https://i.imgur.com/ZoNDdPU.png)

```
flop ratio = (number of flip flops) / (number of total cells)
```
- From the above stata we can absorve that,flip flop ratio is 10.84%.
- After completing the synthesis process, we can now confirm that all the mappings have been performed by ABC.
![image](https://i.imgur.com/oQIMLsn.png)
-In the report, we can identify the completion time of the actual synthesis. The synthesis statistics report displayed below is consistent with the previously observed information.


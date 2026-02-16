# Tessent-Commands
This repository provides scripts and command sequences for implementing DFT flows including scan insertion, ATPG, violation fixing, and test point insertion

## Scan Insertion 

```
set_context dft -scan
read_verilog design.v
read_cell_library atgplib.mdt 
set_current_design <top_module>
set_design_level <top, chip, physical_block, sub_block or instrument_block>
analyze_control_signals -auto
#add_clocks 0 clk
#add_clocks 1 reset
check_design_rules
set_scan_insertion_options -chain_count 10 -chain_length 10
analyze_scan_chains 
insert_test_logic -write_in_tsdb on 

report_scan_elements > ../outputs/scan_elements.txt
report_scan_chains > ../outputs/scan_chains.txt
report_scan_cells > ../outputs/scan_cells.txt
report_scan_enable > ../outputs/scan_enable.txt

write_design -output_file ../outputs/scan.v -replace
write_atpg_setup design_ATPG
set_system_mode setup
open_visualizer
```

## ATPG

```
set_context patterns -scan
read_cell_library slow.atpglib
read_verilog scan_inserted_netlist.v
set_current_design <top_module>
add_black_boxes -auto
set_edt_options off
dofile ATPG_*.dofile 
tessent_scan_setup
set_system_mode analysis
set_fault_type transition
add_faults -all
create_patterns
```

## Create new port
```
set_system_mode insertion
create_port <port_name> -direction input/output
```
```
#example
set_system_mode insertion
create_port test_mode -direction input
create_poer scan_enable -direction input
set_system_mode setup
add_input_constraints -c1 test_mode
set_scan_enable scan_enable -active high
```

## Add reset synchronizers 
```
set_test_logic -reset on -set on -clock on
```


## test-point insertion
```
analyze_test_points
insert_test_points
```

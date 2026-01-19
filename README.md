# Tessent-Commands
This repository provides scripts and command sequences for implementing DFT flows including scan insertion, ATPG, violation fixing, and test point insertion

## Scan Insertion 

```
set_context dft -scan
read_verilog design.v
read_cell_library atgplib.mdt 
set_current_design <top_module>
analyze_control_signals -auto
check_design_rules
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



## Add reset synchronizers 
```
set_test_logic -reset on -set on -clock on
```


## test-point insertion
```
analyze_test_points
insert_test_points
```

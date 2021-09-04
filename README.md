# RTL design using Verilog with SKY130 Technology
### Date: 1 - 5 September 2021
![Verilog-flyer](https://user-images.githubusercontent.com/89923461/131783295-c3fc9552-54ea-4c48-94eb-b773fe9f84e5.png)

# Day 1 - Introduction to Verilog RTL design and Synthesis

# 1- Introduction to iVerilog design test bench

## Required Terminologies 
### Simulator
A Simulator is a tool for checking the RTL design which is designed according to the given specifications. In this workshop we used a open-source simulator called *iVerilog* for the design simulations. To know more about iVerilog, click [here](http://iverilog.icarus.com/).

### Design
The Verilog code created according with the given specification to meet certain functionalities is a *Design*.

### Testbench
Testbench is a setup which applies stimulus to the design and match the output according to the given specifications. Testbench (*test_vectors*) essentially checks the function of the given design using stimulus generator and observer.

This is the design and testbench setup with primary inputs and outputs via stimulus generators and observers. Here the design can have more than one primary inputs and output, but note that *testbench does not have any*.

![P1](https://user-images.githubusercontent.com/89923461/131779349-6e860982-6060-44fb-afea-ca74ac64fc1f.jpg)


## How Simulator Works?
Simulator looks for the changes in the input and for every single input change, the output will be evaluated. Simulator will not evaluate if there is no in input.

## iVerilog based Simulation Flow
The design and the testbench is applied to the simulator *iVerilog*. As we already know the simulator looks for the changes in the input and dumps the changes in the output as a *.vcd* file.(*.vcd stands for Value change dump format*). To view this *.vcd* file, we can use the *gtkwave* tool for viewing the waveform and verify the function of the design.

![P2](https://user-images.githubusercontent.com/89923461/131781245-73a1a50b-c52b-4e69-9b72-ca53cad281c6.jpg)

# 2- Labs using iVerilog and gtkwave

## Introduction to Lab
Before staring the simulation, we must setup the essential files fot the lab. First of all we must create a directory called ***VLSI*** using the terminal.

`mkdir VLSI`

After creating the VLSI directory, we must *git clone* some repositories through the given commands.

For vsdflow, `git clone https://github.com/kunalg123/vsdflow.git`

For RTL Design and Synthesis Workshop, `git clone https://github.com/kunalg123/sky130RTLDesignAndSynthesisWorkshop.git`

After git cloning we can see that two new directories (*sky130RTLDesignAndSynthesisWorkshop* and *vsdflow*) have been created in the VLSI directory. To see all the directories in the VLSI, type `ls` command.

![P3](https://user-images.githubusercontent.com/89923461/131804662-ce517e6c-3654-495e-b8b6-6271630699c5.jpg)

By changing directory using `cd` command, we can change the directory to *sky130RTLDesignAndSynthesisWorkshop* and view the files in *verilog_files* directory. From the list of files, we can see that there are files with a ***file name.v*** and also a file with name ***tb_file_name.v***. Those files are the design file and the corresponding testbench file respectively.

![P4](https://user-images.githubusercontent.com/89923461/131805428-26b84e20-8ec4-4720-8c28-50785413160f.jpg)

Now, let us view a verilog file using the command `gvim <verilog_file.v>`. For example let us take multiplexer design,

`gvim tb_good_mux.v`

Now a editor will be opened displaying the verilog testbench code for the multiplexer.

```
`timescale 1ns / 1ps
module tb_good_mux;
	// Inputs
	reg i0,i1,sel;
	// Outputs
	wire y;

        // Instantiate the Unit Under Test (UUT)
	good_mux uut (
		.sel(sel),
		.i0(i0),
		.i1(i1),
		.y(y)
	);

	initial begin
	$dumpfile("tb_good_mux.vcd");
	$dumpvars(0,tb_good_mux);
	// Initialize Inputs
	sel = 0;
	i0 = 0;
	i1 = 0;
	#300 $finish;
	end

always #75 sel = ~sel;
always #10 i0 = ~i0;
always #55 i1 = ~i1;
endmodule

```

By using the same steps and command we can view the tools installed in the *vsdflow* directory.

![P5](https://user-images.githubusercontent.com/89923461/131805741-695fc4d3-bf2f-47e6-af9a-31973dbd9da8.jpg)

## Simulation using iVerilog and gtkwave

To simulate the verilog design using iVerilog, we must write the command 

`iverilog <file_name.v> <tb_file_name.v>`

For example, lets take a multiplexer design (***good_mux.v***) from the directory and simulate using the command,

`iverilog good_mux.v tb_good_mux.v`

After the command, there will be a file (***a.out***) created in the *verilog_files* directory.

![P6](https://user-images.githubusercontent.com/89923461/131810257-f6d04c7f-60fa-4f23-bacc-10feeb81303f.jpg)

Now we must execute the *a.out* file using `./a.out`, which eventually dumps the *.vcd* file

```
ramachandra@rtlworkshop:~/Desktop/VLSI/sky130RTLDesignAndSynthesisWorkshop/verilog_files$ ./a.out
VCD info: dumpfile tb_good_mux.vcd opened for output.
```
The resulted *tb_good_mux.vcd* file can be viewed through waveform viewing tool, ***gtkwave***. To launch the *gtkwave* tool, the command is

`gtkwave tb_good_mux.vcd`

After launching the *gtkwave* simulator, under the uut (*unit under test*) we can drag the inputs and output signals ( *i0, i1, sel, y*). Adjust the time by zoom fit to view the entire 300 ns to see input and output waveform.

![P7](https://user-images.githubusercontent.com/89923461/131813333-97d1d7d3-50bc-45ff-9fe7-aa29c2d635b4.jpg)

# 3- Yosys and Logic Synthesis

## Introduction to Yosys

Synthesizer is a tool for the conversion of RTL Design to Netlist. For this workshop, we are going to use the open-source synthesizer, *Yosys*. When *.lib* and Design are applied to the Yosys, Netlist of the design is resulted.

![P8](https://user-images.githubusercontent.com/89923461/131818699-99fe95e0-6c41-43c7-94ee-fb271335a6dd.jpg)

To verify the synthesis, we must simulate using *iVerilog* by loading the netlist and testbench. Get that resulted *.vcd* file and simulate and observe the waveform using the *gtkwave* tool.

>Note: Stimulus provided must be same as output observed during RTL design simulation. We can use the same testbench which we used for the RTL design simulation.

![P9](https://user-images.githubusercontent.com/89923461/131820299-803b0ad8-0bf2-4ccc-8a99-4c8c1a912912.jpg)

## RTL Design

RTL Design is the behavioral representation of the required specification in a HDL (*Hardware Description Language*). This is what a RTL design code in Verilog looks like,

```
module sample_code (input clk,rst, output result,done);

always @ (posedge clk, posedge rst)
  if (rst)
    -----------
  else
    -----------
endmodule

```
## Synthesis

Synthesis is the RTL design to the gate level translation, the *design  code* which we saw above will be converted to hardware logic design with gates. Netlist is the output of the Synthesis.

![P10](https://user-images.githubusercontent.com/89923461/131937809-243cc4b0-a9bc-412a-9f7c-65b85f88a9a2.jpg)

## What is .lib?

.lib is the collection of the logical modules which consists of the logic gates such as AND, OR, NOT ..... etc. with different flavours( speeds of execution) which suits the design aspect. *.lib* contains these standard cells to implement any boolean logic.

## Why different flavours of gate? 

In a digital logic circuit, the delay of the combination circuit in it determines the maximum speed of operation of the whole circuit. For example, lets look at the setup with two flip flops

![P11](https://user-images.githubusercontent.com/89923461/132020384-e65f0447-58e1-4a3a-8c86-4b5122b2160b.jpg)

Here in this setup, to know about the maximum clock frequency (*minimum clock period*) to be applied on this circuit, we must know that it depends upon the propagation delay of FF A, combinational circuit and the setup time of the FF B. In order to get minimum clock period, we must have faster cells. 

## Why do we need Slow cells?

If we should not violate the hold time of the second flip flop, we must use slower cells. The minimum hold time depends upon propagation delay of FF A and combinational circuit. Essentially to avoid the *HOLD* issues, we need the slower cells.

![P12](https://user-images.githubusercontent.com/89923461/132077196-57c2ff41-4bd9-4e36-8acf-b2a260233b18.jpg)

For conclusion we can say that, for maximum performance we need faster cells and to avoid *HOLD* issues we need slower cells. *.lib* fulfills these issues by having the collection of both type of cells.

## Faster vs Slower cells

In a digital circuit, capacitance acts as the load for the circuit. So the delay depends upon the charging and discharging time of the capacitor, faster charging leads to less delay and slower charging and discharging leads to more delay. If we need faster cells, then the transistor used must be capable of sourcing more power.

Wider transistors promises us less delay but at the cost of more area and power. Narrow transistors shows less delay with less area and power. 

## Selection of cells

We need to guide the synthesizer to select the different types of cells, which suits the effective implementation of our digital circuit. We know that, faster cells must have more power and area and violates hold time.
In the other hand, Slower cells do not meet the required performance. So, we must provide ***constraints*** to the synthesizer in order to guide the selection of the cells. 

Now let us look at the illustration of a synthesis of a RTL design,

![P13](https://user-images.githubusercontent.com/89923461/132078192-c9ec44ec-9629-4841-8c1d-ad0662d84215.jpg)

# 4 - Labs using Yosys and SKY130 PDKs

## Yosys for Synthesis

Before invoking yosys we must in the directory *verilog_file*. To invoke yosys, just type `yosys`

![P14](https://user-images.githubusercontent.com/89923461/132078442-2957c1f1-dcff-4faf-b984-b74a9906d057.jpg)

In the above screenshot, we can see that after the `yosys` command, the *yosys* prompt is opened. Now the first thing to do is read the library.

`yosys> read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib`

```
1. Executing Liberty frontend.
Imported 428 cell types from liberty file.
```

Now lets read a verilog file (*good_mux.v*) by using the command,

`read_verilog good_mux.v`

After typing the command, we can see a result `Sucessfully finished Verilog Frontend`

![P15](https://user-images.githubusercontent.com/89923461/132078765-a34840af-65d4-4d8d-b8fe-b510dda71041.jpg)

Now to synthesize the *good_mux* module, the command is

`synth -top good_mux`

![P16](https://user-images.githubusercontent.com/89923461/132078859-ef7d036c-0e27-47dc-9eb0-597aaa6ad572.jpg)

Now to generate the netlist, the command is

`abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib `

![P17](https://user-images.githubusercontent.com/89923461/132078996-289cc428-3656-45d9-8835-9b5b5a462bab.jpg)

From the above screenshot, we can see that the RTL design is converted into the standard cell design from the library file we mentioned in the command. Now if we look a little close we can see that it has shown the input and output signals,

```
4.1.2. Re-integrating ABC results.
ABC RESULTS:   sky130_fd_sc_hd__clkinv_1 cells:        1
ABC RESULTS:   sky130_fd_sc_hd__nand2_1 cells:        1
ABC RESULTS:   sky130_fd_sc_hd__o21ai_0 cells:        1
ABC RESULTS:        internal signals:        0
ABC RESULTS:           input signals:        3
ABC RESULTS:          output signals:        1
```

Now if we want to see the logic realized by the circuit, type `show`

```
5. Generating Graphviz representation of design.
Writing dot description to `/home/ramachandra/.yosys_show.dot'.
Dumping module good_mux to page 1.
Exec: { test -f '/home/ramachandra/.yosys_show.dot.pid' && fuser -s '/home/ramachandra/.yosys_show.dot.pid'; } || ( echo $$ >&3; exec xdot '/home/ramachandra/.yosys_show.dot'; ) 3> '/home/ramachandra/.yosys_show.dot.pid' &
```

![P18](https://user-images.githubusercontent.com/89923461/132079135-c1696ebf-ec38-4230-a3b0-e1c11a6fe55c.jpg)

From the above screenshot, we can see the graphical representation of the *good_mux* using a NOT, OR and a 2 - input NAND gates. For even simpler representation of the gates,

![P19](https://user-images.githubusercontent.com/89923461/132079671-96bfd13c-4a18-4d82-994d-736d48274f12.jpg)

LAB 3

# Day 2 - Timing libs, hierarchial vs flat synthesis efficient flop coding styles

# 1 - Introduction to timing .libs

## .lib 

We already know about .lib, but now lets look what it looks like in an editor







































# Day 3 - Combinational and Sequential Optimizations

# Day 4 - GLS, blocking vs non-blocking and Synthesis-Simulation mismatch

# Day 5 - If, case, for loop and for generate






























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

## Introduction to Yosys




























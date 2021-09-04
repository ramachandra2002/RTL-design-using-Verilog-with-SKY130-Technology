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

The command to write the netlist is,

`write_verilog good_mux_netlist.v`

```
6. Executing Verilog backend.
Dumping module `\good_mux'.
```

To open the netlist,

`!gvim good_mux_netlist.v`

![P20](https://user-images.githubusercontent.com/89923461/132080171-6d45e56d-0817-4aa7-ac2c-6b57fbc4d064.jpg)

```
/* Generated by Yosys 0.7 (git sha1 61f6811, gcc 6.2.0-11ubuntu1 -O2 -fdebug-prefix-map=/build/yosys-OIL3SR/yosys-0.7=. -fstack-protector-strong -fPIC -Os) */

(* top =  1  *)
(* src = "good_mux.v:2" *)
module good_mux(i0, i1, sel, y);
  wire _0_;
  wire _1_;
  wire _2_;
  wire _3_;
  wire _4_;
  wire _5_;
  (* src = "good_mux.v:2" *)
  input i0;
  (* src = "good_mux.v:2" *)
  input i1;
  (* src = "good_mux.v:2" *)
  input sel;
  (* src = "good_mux.v:2" *)
  output y;
  sky130_fd_sc_hd__clkinv_1 _6_ (
    .A(_0_),
    .Y(_4_)
  );
  sky130_fd_sc_hd__nand2_1 _7_ (
    .A(_1_),
    .B(_2_),
    .Y(_5_)
  );
  sky130_fd_sc_hd__o21ai_0 _8_ (
    .A1(_2_),
    .A2(_4_),
    .B1(_5_),
    .Y(_3_)
  );
  assign _0_ = i0;
  assign _1_ = i1;
  assign _2_ = sel;
  assign y = _3_;
endmodule
```

To get a even more simplified netlist, the command is 

`write_verilog -noattr good_mux_netlist.v`

```
7. Executing Verilog backend.
Dumping module `\good_mux'.
```

Now if we type `!gvim good_mux_netlist.v`, the simplified netlist can be viewed.

![P21](https://user-images.githubusercontent.com/89923461/132080509-cf85f3b5-f3f2-40a2-8f70-f7071e86f9ed.jpg)

# Day 2 - Timing libs, hierarchial vs flat synthesis efficient flop coding styles

# 1 - Introduction to timing .libs

## .lib 

We already know about .lib, but now lets look what it looks like in an editor. After going to the *verilog_files* directory, type

`gvim ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib`

![P1](https://user-images.githubusercontent.com/89923461/132082501-c720d52d-f3ad-402c-b21b-13321a4d5e74.jpg)

>Note: To off the red overlay on the lines, type `:syn off`. This disables the red highlighted syntax.

![P2](https://user-images.githubusercontent.com/89923461/132082903-a46219f9-73f7-446b-91d4-e185fa7e76ea.jpg)

If we go down the library file we see many lines mentioning *cell*, with different flavours. To search for the *cell* type, `/cell ` and `:g//`

![P3](https://user-images.githubusercontent.com/89923461/132083153-1db2e572-687c-47ca-bbd7-e17c620ff305.jpg)

## Inside a standard cell

Now lets look inside a standard cell in the library, for example let us search for a AND gate with the command `/cell .*and`

![P4](https://user-images.githubusercontent.com/89923461/132083330-99344090-1173-46dd-a35c-aae985cfc7eb.jpg)

If we look closer we can see the four combination of the data for the two inputs present in the 2 input AND gate

![P5](https://user-images.githubusercontent.com/89923461/132083504-bbc49980-db3b-413f-b9f2-53aa1fac3270.jpg)

Now lets compare two AND gates with a difference in the value of area

![P6](https://user-images.githubusercontent.com/89923461/132083630-57b04575-137b-4398-8eea-4018b3e3d1e1.jpg)

From the above screenshot, we can clearly see that ***cell("sky130_fd_sc_hd_and2_2")*** has more area than the ***cell("sky130_fd_sc_hd_and2_0")***. Now for better understanding in the comparison let us take the three flavours of the same cell 2 input AND.

![P7](https://user-images.githubusercontent.com/89923461/132083834-f491e93a-3fc5-458a-9e00-053da090d47a.jpg)

We can see that for the three cells ( ***cell("sky130_fd_sc_hd_and2_0"), cell("sky130_fd_sc_hd_and2_2"), cell("sky130_fd_sc_hd_and2_4")***) the area is increased and power is also increased.

# 2 - Hierarchial vs Flat Synthesis

The file we are going to synthesize is *multiple_modules*. Lets open it by `gvim multiple-modules.v`

![P8](https://user-images.githubusercontent.com/89923461/132084439-34fcb471-14c7-4197-aae2-b7551dba4bf2.jpg)

This is the multiple_modules.v file with two sub modules (OR and AND ), the main module instantiates the both modules within it. Now let us synthesis the file using yosys (*we saw how to synthesize earlier. To know about the synthesizing process click [here](https://github.com/ramachandra2002/RTL-design-using-Verilog-with-SKY130-Technology#yosys-for-synthesis)*). After the `synth -top multiple_modules`

![P9](https://user-images.githubusercontent.com/89923461/132084772-7a53636c-2aa7-4ca2-8578-a27b2e777d3d.jpg)

![P10](https://user-images.githubusercontent.com/89923461/132084880-b2a9400b-5491-48ab-9b42-9b2c850eb48d.jpg)

After the `show multiple_modules` command,

![P11](https://user-images.githubusercontent.com/89923461/132084914-ab7c4767-38ee-4185-a405-50de854285be.jpg)

![P12](https://user-images.githubusercontent.com/89923461/132084930-c50a70c1-bae7-448c-af8f-e607cea76bf1.jpg)

We can see the modules itself inside the multiple_modules instance instead of the AND and OR gates. Here the hierarchies are preserved, so it is called as *Hierarchial design*.
Now let us look the netlist generated from this design,

`write_verilog -noattr multiple_modules_hier.v'

`!gvim  multiple_modules_hier.v`

![P13](https://user-images.githubusercontent.com/89923461/132085118-9cd83d39-8f92-45c7-a05c-2179985b1ef7.jpg)

From the above image, we can see that the hierarchy is preserved. Now lets generate a flat netlist using the command `flatten`

![P14](https://user-images.githubusercontent.com/89923461/132085208-56c0b8b3-5f4a-4a00-b46b-d41eb2ac7179.jpg)

`write_verilog multiple_modules_flat.v`

![P15](https://user-images.githubusercontent.com/89923461/132085250-2e1a48f1-3fe0-4258-a76c-5f8452a26ec2.jpg)

`!gvim multiple_modules_flat.v`

![P16](https://user-images.githubusercontent.com/89923461/132085279-7bf0401a-114c-4b79-9355-0f179c06782e.jpg)

Now for comparison of both synthesis, use `:vsp` command

![P17](https://user-images.githubusercontent.com/89923461/132085344-8e373991-9514-4973-be1d-1656c9294ca4.jpg)

We can the direct instantiation of OR and AND gates in the flattened out netlist instead of the hierarchial design. Let us see the synthesis of the flat design by typing `show` command,

![P18](https://user-images.githubusercontent.com/89923461/132085455-e7c7f920-bebd-4513-bbfd-3552f3bfecfd.jpg)

![P19](https://user-images.githubusercontent.com/89923461/132085456-d978cd3f-7724-4bcf-ad9b-37427cc933ed.jpg)

# 3 - Flops and Flop coding styles

Let us observe and simulate the flop styles. First lets simulate the file *dff_asyncres.v* using iVerilog,

`iverilog dff_asyncres.v tb_dff_asyncres.v`

`./a.out`

![P20](https://user-images.githubusercontent.com/89923461/132085938-c5ae71a5-5aae-4e98-b7fb-d98f9c6f763d.jpg)

`gtkwave tb_dff_asyncres.vcd`

![P21](https://user-images.githubusercontent.com/89923461/132086016-9e24c504-e482-4e94-a7fa-e5eefb5ce045.jpg)

![P22](https://user-images.githubusercontent.com/89923461/132086028-282ed27c-e872-4e38-8844-e67ab9350664.jpg)

Now let us look the file *dff_async_set.v* after launching gtkwave,

![P23](https://user-images.githubusercontent.com/89923461/132086175-f35e3f2d-8428-4bb2-8678-f37ffa210490.jpg)

![P24](https://user-images.githubusercontent.com/89923461/132086184-1c398ca6-ac96-4f11-a2e3-2f3e98f04f56.jpg)

Now let us look the file *dff_syncres.v* after launching gtkwave,

![P25](https://user-images.githubusercontent.com/89923461/132086304-b90b4a2c-fd2f-45d2-8ecb-630fa8d0cb96.jpg)

![P26](https://user-images.githubusercontent.com/89923461/132086310-50ce6f8f-9fcf-4f2b-b623-ffaca16ee6ac.jpg)

Now let us synthesis the ff design using yosys,

![P27](https://user-images.githubusercontent.com/89923461/132086868-054f584f-2024-402f-9c73-a05146ceaf55.jpg)

After typing `synth -top dff_asyncres `

![p28](https://user-images.githubusercontent.com/89923461/132086921-112f3fc2-6923-4098-8ae4-b3cdca407d1c.jpg)

`dfflibmap -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib`

![p29](https://user-images.githubusercontent.com/89923461/132086981-67b1770c-9681-4528-8c64-0f9d3ee3f7b8.jpg)

`abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib`

![p30](https://user-images.githubusercontent.com/89923461/132087028-fd48fa94-ac2a-498f-b26c-2e687e8fe31b.jpg)

Now let type the `show` command,

![p31](https://user-images.githubusercontent.com/89923461/132087080-5bd1627e-e9f1-4f80-b6e6-a2213c2e0aaa.jpg)

![p32](https://user-images.githubusercontent.com/89923461/132087089-721534fc-bacb-4824-be90-03e64dc1a51c.jpg)

# Day 3 - Combinational and Sequential Optimizations

## Combinational Logic Optimization

In todays lab session we were going to do synthesis based on optimization. We have to search for opt related files,

![p1](https://user-images.githubusercontent.com/89923461/132087315-edf27e24-223c-4be1-b601-7b1b7bc7f520.jpg)

`gvim opt_check.v`

![p2](https://user-images.githubusercontent.com/89923461/132087373-50731883-58ff-4d19-945e-fb0946946a28.jpg)

Now lets synthesize the file using yosys,

` read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib`

![p3](https://user-images.githubusercontent.com/89923461/132087437-1445b11d-fafb-4ba0-a84d-39bf10e5215d.jpg)

`read_verilog opt_check.v`

![p4](https://user-images.githubusercontent.com/89923461/132087481-94e05faf-ce5e-4a2d-9487-dc90e1c449c6.jpg)

'synth -top opt_check`

![p5](https://user-images.githubusercontent.com/89923461/132087520-861dfeb5-f89f-453c-a4a1-a2ee92eecd98.jpg)

The command for the constant propagation and optimization 

`opt_clean -purge`

![p6](https://user-images.githubusercontent.com/89923461/132087580-6b2d3328-d130-430f-a563-33b52ee1d6b4.jpg)

`abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib`

![p7](https://user-images.githubusercontent.com/89923461/132087614-a971736f-91b4-4427-afca-0e562c85892e.jpg)

After the `show` command,

![p8](https://user-images.githubusercontent.com/89923461/132087650-9ff59377-439d-46ef-96a5-cdba7679fe60.jpg)

![p9](https://user-images.githubusercontent.com/89923461/132087657-8daa0584-8538-4af0-9438-81f20ea169ef.jpg)

From the above image we can see that after synthesis it has been optimized to an AND gate.

## Sequential Logic Optimization 

By using similar steps from the above synthesis, we are going to synthesize the file *dff_const1.v*

![p10](https://user-images.githubusercontent.com/89923461/132088274-e1cc707f-d004-4d88-b213-0acd6757631f.jpg)

![p11](https://user-images.githubusercontent.com/89923461/132088275-f8b7d437-26ae-48fc-9b63-bf5c93c976c4.jpg)

![p12](https://user-images.githubusercontent.com/89923461/132088276-96f9e9d2-d34c-4aa3-aa55-13a277d19b54.jpg)

![p13](https://user-images.githubusercontent.com/89923461/132088277-8de005c4-894d-4cb7-b908-650101092e47.jpg)

![p14](https://user-images.githubusercontent.com/89923461/132088278-87f61664-6db6-4b82-ba26-04c590288993.jpg)

![p15](https://user-images.githubusercontent.com/89923461/132088273-a86ff99a-f8f7-4a2f-bc93-ef32e187492c.jpg)

# Day 4 - GLS, blocking vs non-blocking and Synthesis-Simulation mismatch

Gate Level Simulation is run for verifiction of logic in the circuit and to check the timings and the GLS must be run with delay annotation. 

## Synthesis - Simulation Mismatch

There is need for verification because of this synthesis - simulation mismatch. Major reasons for this mismatch are,
 * missing sensitivity list
 * blocking & non-blocking assignments

## Missing Sensitivity List

The simulator generally works based on the activity, in other words the change in inputs will initiate to get the outputs. For example, let take a MUX 

```
module mux(input i0, input i1, input sel,output reg y)

always@(sel)
begin
  if(sel)
    y=i1;
  else
    y=i0;
end
endmodule
```
From the above code snippet, the always block works only when the select is high. So it is insensitive to the other inputs such as i0 and i1. Once there is a change in the sel value the i1 values will be assigned to the y, but the changes of i1 will not be reflected in the output y. So that is wrong and insensitive. This is called as missing elements of selectivity list.

The correct way of writing the code is,

```
module mux(input i0, input i1, input sel,output reg y)

always@(*)
begin
  if(sel)
    y=i1;
  else
    y=i0;
end
endmodule

```
Here in this code, `always@(*)` the star means the always block will be executed whenever there is a change in the signals.

## Blocking and Non-Blocking Assignments

These two assignments are always written inside the blocks. Non blocking assignments does parallel evaluation, executes RHS and will assign the same to the LHS. Blocking Assignments executes the code in the same order of writing.

## Labs on Synthesis Simulation Mismatch

Lets take the file `tb_ternary_operator_mux.v` for demonstration. After the iVerilog, gtkwave wave simulation,

```
iverilog ternary_operator_mux.v tb_ternary_operator_mux.v 
./a.out
gtkwave tb_ternary_operator_mux.vcd
```
![p1](https://user-images.githubusercontent.com/89923461/132090788-ab4e0789-90ae-44dc-8254-ed551827795a.jpg)

![p2](https://user-images.githubusercontent.com/89923461/132090806-3bf05f56-c763-45ce-9f39-b618d8624ec9.jpg)

Now lets synthesize the design using yosys,

```
read_liberty -lib ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
read_verilog ternary_operator_mux.v
synth -top ternary_operator_mux
abc -liberty ../my_lib/lib/sky130_fd_sc_hd__tt_025C_1v80.lib
write_verilog -noattr ternary_operator_mux_net.v
show
```
![p3](https://user-images.githubusercontent.com/89923461/132091110-5aed3844-eb6d-45d6-9674-5121d02ae8e8.jpg)

![p4](https://user-images.githubusercontent.com/89923461/132091133-cf64f2eb-6b98-4b7a-bbd3-bb40985155eb.jpg)

To demonstrate the mismatch between the synthesis and simulation, let us take a example of *bad_mux.v*,

![p5](https://user-images.githubusercontent.com/89923461/132091393-f0d995e0-cce7-4622-b560-7c269579b68b.jpg)

![p6](https://user-images.githubusercontent.com/89923461/132091433-0d768e5f-656b-4541-ab1c-017b0b666007.jpg)

![p7](https://user-images.githubusercontent.com/89923461/132091452-b819a2b6-3155-4eee-98dc-0be4b5ee7079.jpg)

The above screenshot depicts the RTL simulation of the bad_mux design. Now let us synthesize and the write the file to Gate level simulation,

![p8](https://user-images.githubusercontent.com/89923461/132091811-9c117792-7e2c-4ad9-af22-73e7d3a99043.jpg)

![p9](https://user-images.githubusercontent.com/89923461/132091805-1ce167ba-5457-4852-bbea-cd542cfcb846.jpg)

![p10](https://user-images.githubusercontent.com/89923461/132091806-014a3e2c-28db-4271-9254-8cd37ed9cbf6.jpg)

![p11](https://user-images.githubusercontent.com/89923461/132091807-bc165372-09f6-4c18-8d3d-8849a9a31536.jpg)

![p12](https://user-images.githubusercontent.com/89923461/132091809-f90a57b6-4903-492f-8c02-716ddd064b8b.jpg)

After performing GLS, there will be a difference in the waveform graphs between the RTL simulation and GLS simulated one.

## Non-Blocking vs Blocking Synthesis-Simulation Mismatch

`iverilog blocking_caveat.v tb_blocking_caveat.v`

`./a.out`

`gtkwave tb_blocking_caveat.vcd	`

The RTL simulation of the file,

![p13](https://user-images.githubusercontent.com/89923461/132092113-055556b8-8adc-4805-9c7f-05350098c0b7.jpg)

![p14](https://user-images.githubusercontent.com/89923461/132092147-189dcd64-6436-46a4-9f46-1a64b5ee1033.jpg)

Now lets synthesis using yosys,

![p15](https://user-images.githubusercontent.com/89923461/132092350-a521c188-c0ae-4424-8415-4af3fda42daa.jpg)

![p16](https://user-images.githubusercontent.com/89923461/132092344-4436b5db-06a9-4833-ae65-01b61c417128.jpg)

![p17](https://user-images.githubusercontent.com/89923461/132092346-7c48d5dd-1277-48d6-ae01-ef4a245692a5.jpg)

![p18](https://user-images.githubusercontent.com/89923461/132092347-d93a65ec-83a9-4e7a-ad44-1d62648a1fec.jpg)

![p19](https://user-images.githubusercontent.com/89923461/132092349-a624745c-b603-405c-a1d2-292becc584ca.jpg)

After performing GLS,

![p20](https://user-images.githubusercontent.com/89923461/132092658-13505af8-71cd-4f04-a376-13d0833ede29.jpg)

![p21](https://user-images.githubusercontent.com/89923461/132092709-bfafed83-f952-4553-a470-43301e0e5883.jpg)

![p22](https://user-images.githubusercontent.com/89923461/132092710-0a2d3d29-cda3-4c58-9878-9d6adfae1069.jpg)

We can see the clear difference between the RTL and GLS simulations of the design.

# Day 5 - If, case, for loop and for generate

## If else constructs

This is a general if else construct,

```
if(condtion1)
 -----
elseif(condition2)
 -----
else
 -----
```

Lets start with incomplete if,

![p1](https://user-images.githubusercontent.com/89923461/132092974-d68f738f-84af-4fbc-9091-3fee9694aeee.jpg)

![p2](https://user-images.githubusercontent.com/89923461/132092971-7b4c08de-aefa-46b0-b688-6ab0b95c6645.jpg)

Lets see it waveform,

![p3](https://user-images.githubusercontent.com/89923461/132093104-1335ca28-3a9c-4965-987a-e8accfed0bb9.jpg)

## For loop and For generate

For the understanding let us use MUX in the form of for loop,

![p4](https://user-images.githubusercontent.com/89923461/132093398-7c222d92-0173-4b77-8697-9a80c669a35b.jpg)

![p5](https://user-images.githubusercontent.com/89923461/132093399-e340560a-4c60-4adb-ab54-3d26d2652609.jpg)

![p6](https://user-images.githubusercontent.com/89923461/132093394-99fde1ed-024c-43a6-8954-9316d6dec9f6.jpg)

![p7](https://user-images.githubusercontent.com/89923461/132093397-98f310a6-8e27-4e48-923b-774acd78dc11.jpg)

To understand generate statements lets find an application using Ripple carry adder,



































# RTL-design-using-Verilog-with-SKY130-Technology
## Day 1 - Introduction to Verilog RTL design and Synthesis

### Required Terminologies 
#### Simulator
A Simulator is a tool for checking the RTL design which is designed according to the given specifications. In this workshop we used a open-source simulator called *iVerilog* for the design simulations. To know more about iVerilog, click 
[here] (http://iverilog.icarus.com/)

#### Design
The Verilog code created according with the given specification to meet certain functionalities is a *Design*.

#### Testbench
Testbench is a setup which applies stimulus to the design and match the output according to the given specifications. Testbench (*test_vectors*) essentially checks the function of the given design using stimulus generator and observer.

This is the design and testbench setup with primary inputs and outputs via stimulus generators and observers. Here the design can have more than one primary inputs and output, but note that *testbench does not have any*.

![P1](https://user-images.githubusercontent.com/89923461/131779349-6e860982-6060-44fb-afea-ca74ac64fc1f.jpg)


### How Simulator Works?
Simulator looks for the changes in the input and for every single input change, the output will be evaluated. Simulator will not evaluate if there is no in input.

### iVerilog based Simulation Flow
The design and the testbench is applied to the simulator *iVerilog*. As we already know the simulator looks for the changes in the input and dumps the changes in the output as a *.vcd* file.(*.vcd stands for Value change dump format*). To view this *.vcd* file, we can use the *gtkwave* tool for viewing the waveform and verify the function of the design.

![P2](https://user-images.githubusercontent.com/89923461/131781245-73a1a50b-c52b-4e69-9b72-ca53cad281c6.jpg)






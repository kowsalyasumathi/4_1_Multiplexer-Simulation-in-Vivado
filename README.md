# SIMULATION AND IMPLEMENTATION OF 4:1 MULTIPLEXER

## AIM
To design and simulate a 4:1 Multiplexer (MUX) using Verilog HDL in four different modeling styles—Gate-Level, Data Flow, Behavioral, and Structural—and to verify its functionality through a testbench using the Vivado 2023.1 simulation environment. The experiment aims to understand how different abstraction levels in Verilog can be used to describe the same digital logic circuit and analyze their performance.

## APPARATUS REQUIRED
- **Vivado 2023.1**

## Procedure

1. Open **Vivado 2023.1**.  
2. Create a **New RTL Project** and give a name (e.g., `Mux4_to_1`).  
3. Add/create your Verilog files and testbench.  
4. Select an FPGA part (e.g., `xc7a35ticsg324-1L`).  
5. Run **Synthesis** to check for errors.  
6. Run **Simulation** → **Run Behavioral Simulation**.  
7. Observe the waveforms of inputs and outputs.  
8. Adjust simulation time if needed (e.g., 1000ns).  
9. Save the project and take screenshots of results.  
10. Close simulation.  

---

## Logic Diagram
![image](https://github.com/user-attachments/assets/d4ab4bc3-12b0-44dc-8edb-9d586d8ba856)

---

## Truth Table
![image](https://github.com/user-attachments/assets/c850506c-3f6e-4d6b-8574-939a914b2a5f)

---

## Verilog Code

### 4:1 MUX Gate-Level Implementation
```verilog
module mux_41(I,S,Y);
input [3:0]I;
input[1:0]S;
output Y;
wire [4:1]W;
and g1(W[1],(~S[1]),(~S[0]),I[0]);
and g2(W[2],(~S[1]),S[0],I[1]);
and g3(W[3],S[1],(~S[0]),I[2]);
and g4(W[4],S[1],S[0],I[3]);
endmodule

```
### 4:1 MUX Gate-Level Implementation- Testbench
```verilog
`timescale 1ns/1ps
module mux_41_tb;
reg [3:0]I;
reg [1:0]S;
wire Y;
mux_41 uut(I,S,Y);
initial
begin
I=4'B0001;
S=2'b00;
#10
$display("Selection is %b %b , output : %b ", S[1],S[0],Y);
S=2'b01;
#10
$display("Selection is %b %b , output : %b ", S[1],S[0],Y);
S=2'b10;
#10
$display("Selection is %b %b , output : %b ", S[1],S[0],Y);
S=2'b11;
#10
$display("Selection is %b %b , output : %b ", S[1],S[0],Y);
$finish;
end 
endmodule
```
## Simulated Output Gate Level Modelling
<img width="1920" height="1080" alt="Screenshot (336)" src="https://github.com/user-attachments/assets/47b76d9b-a4ab-4545-936e-94b547e6cf9d" />

---
### 4:1 MUX Data flow Modelling
```verilog
module mux41(I,S,Y);
input [3:0]I;
input[1:0]S;
output Y;
wire [4:1]W;
assign W[1]=(~S[0])&(~S[1])&I[0];
assign W[2]=(~S[0])&S[1]&I[1];
assign W[3]=S[0]&(~S[1])&I[2];
assign W[4]=S[0]&S[1]&I[3];
assign Y=W[1]|W[2]|W[3]|W[4];
endmodule
```
### 4:1 MUX Data flow Modelling- Testbench
```verilog
`timescale 1ns/1ps
module mux4_1_tb;
reg [3:0]I;
reg [1:0]S;
wire Y;
mux41 uut(I,S,Y);
initial
begin
I=4'B0110;
S=2'b00;
#10
$display("Selection is %b %b , output : %b ", S[1],S[0],Y);
S=2'b01;
#10
$display("Selection is %b %b , output : %b ", S[1],S[0],Y);
S=2'b10;
#10
$display("Selection is %b %b , output : %b ", S[1],S[0],Y);
S=2'b11;
#10
$display("Selection is %b %b , output : %b ", S[1],S[0],Y);
$finish;
end
endmodule
```
## Simulated Output Dataflow Modelling

<img width="1920" height="1080" alt="Screenshot (332)" src="https://github.com/user-attachments/assets/a0300c2a-8760-4607-8a44-7b0a7c23a383" />


---
### 4:1 MUX Behavioral Implementation
```verilog
module MUX_41(I,S,Y);
input [3:0]I;
input[1:0]S;
output reg Y;
always @(I,S)
    begin
        case(S)
        2'b00:Y=I[0];
        2'b01:Y=I[1];
        2'b10:Y=I[2];
        2'b11:Y=I[3];
        endcase
     end
endmodule

```
### 4:1 MUX Behavioral Modelling- Testbench
```verilog
// Testbench Skeleton
`timescale 1ns/1ps
module mux_41_tb;
reg [3:0]I;
reg [1:0]S;
wire Y;
MUX_41 uut(I,S,Y);
initial
begin
I=4'B1001;
S=2'b00;
#10
$display("Selection is %b %b , output : %b ", S[1],S[0],Y);
S=2'b01;
#10
$display("Selection is %b %b , output : %b ", S[1],S[0],Y);
S=2'b10;
#10
$display("Selection is %b %b , output : %b ", S[1],S[0],Y);
S=2'b11;
#10
$display("Selection is %b %b , output : %b ", S[1],S[0],Y);
$finish;
end 
endmodule
```
## Simulated Output Behavioral Modelling

<img width="1920" height="1080" alt="Screenshot (337)" src="https://github.com/user-attachments/assets/09b385c1-0ad0-4cd0-82cd-95a81320d6da" />


### 4:1 MUX Structural Implementation

![image](https://github.com/user-attachments/assets/eea81c2c-7dfa-43aa-9cea-1ab4ed54db6c)


```verilog
module MUX21(A,B,S,Z);
input A,B,S;
output Z;
wire W1,W2;
and g1(W1,A,~S);
and g2(W2,B,S);
or g3(Z,W1,W2);
endmodule

module MUX_41(I,S,Y);
input [3:0]I;
input [1:0]S;
output Y;
wire [2:1]W;
MUX21 M1(.Z(W[1]),.A(I[0]),.B(I[1]),.S(S[1]));
MUX21 M2(.Z(W[2]),.A(I[2]),.B(I[3]),.S(S[1]));
MUX21 M3(.Z(Y),.A(W[1]),.B(W[2]),.S(S[0]));
endmodule

```
### Testbench Implementation
```verilog
`timescale 1ns / 1ps
module MUX_41_tb;
reg [3:0]I;
reg [1:0]S;
wire Y;
MUX_41 uut(I,S,Y);
initial
begin
I=4'B1000;
S=2'b00;
#10
$display("Selection is %b %b , output : %b ", S[1],S[0],Y);
S=2'b01;
#10
$display("Selection is %b %b , output : %b ", S[1],S[0],Y);
S=2'b10;
#10
$display("Selection is %b %b , output : %b ", S[1],S[0],Y);
S=2'b11;
#10
$display("Selection is %b %b , output : %b ", S[1],S[0],Y);
$finish;
end
endmodule

```
## Simulated Output Structural Modelling

<img width="1920" height="1080" alt="Screenshot (338)" src="https://github.com/user-attachments/assets/b98cee8b-89bb-4e84-b59b-607fa47708f7" />


---
### CONCLUSION

In this experiment, a 4:1 Multiplexer was successfully designed and simulated using Verilog HDL across four different modeling styles: Gate-Level, Data Flow, Behavioral, and Structural.The simulation results verified the correct functionality of the MUX, with all implementations producing identical outputs for the given input conditions.


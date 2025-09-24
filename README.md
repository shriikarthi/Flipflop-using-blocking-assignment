# EXPERIMENT 3: Simulation of All Flip-Flops using Blocking Statement

## AIM
To design and simulate basic flip-flops (SR, D, JK, and T) using **blocking statements** in Verilog HDL, and verify their functionality through simulation in Vivado 2023.1.

## APPARATUS REQUIRED
- Vivado 2023.1
- Computer with HDL Simulator

## DESCRIPTION
Flip-flops are the basic memory elements in sequential circuits.  
In this experiment, different types of flip-flops (SR, D, JK, T) are modeled using **behavioral modeling** with **blocking assignment (`=`)** inside the `always` block.  
Blocking assignments execute sequentially in the given order, which makes it easier to describe simple synchronous circuits.

## PROCEDURE
1. Open **Vivado 2023.1**.  
2. Create a **New RTL Project** (e.g., `FlipFlop_Simulation`).  
3. Add Verilog source files for each flip-flop (SR, D, JK, T).  
4. Add a testbench file to verify all flip-flops.  
5. Run **Behavioral Simulation**.  
6. Observe waveforms of inputs and outputs for each flip-flop.  
7. Verify that outputs match the truth table.  
8. Save results and capture simulation screenshots.

---

## VERILOG CODE

### SR Flip-Flop (Blocking)
```verilog
module sr_ff(s,r,clk,rst,q);
input s,r,clk,rst;
output reg q;
always@(posedge clk)
begin
if (rst==1)
q=1'b0;
else if (s==0 && r==0)
q=q;
else if (s==0 && r==1)
q=1'b0;
else if (s==1 && r==0)
q=1'b1;
else
q=1'bx;
end
endmodule
```
### SR Flip-Flop Test bench 
```verilog
module tb_SRFF;
reg s,r,clk,rst;
wire q;
SRFF uut(s,r,clk,rst,q);
always #5 clk=~clk;
initial begin
clk=0;s=0;r=0;rst=1;
#10 rst=0;
#10 s=0; r=0;
#10 s=0; r=1;
#10 s=1; r=0;
#10 s=1; r=1;
#10 s=0; r=0;
#20 $finish;
end 
endmodule
```
#### SIMULATION OUTPUT

<img width="1919" height="1079" alt="Screenshot 2025-09-17 085240" src="https://github.com/user-attachments/assets/10644f33-9365-4b17-a194-3f8cd7f2c38d" />

---

### JK Flip-Flop (Blocking)
```verilog
module jk_ff(j,k,clk,rst,q);
input j,k,clk,rst;
output reg q;
always@(posedge clk)
begin
if (rst==0)
q=0;
else if(j==0 && k==0)
q=q;
else if(j==0 && k==1)
q <= 1'b0;
else if(j==1 && k==0)
q <= 1'b1;
else
q <= ~q;
end
endmodule
```
### JK Flip-Flop Test bench 
```verilog
module tb_jk_ff;
  reg j, k, clk, rst;
  wire q;
  jk_ff uut (j,k,clk,rst,q);
  always #5 clk = ~clk;
  initial begin
    clk = 0;
    rst = 1;
    j = 0;
    k = 0;
    #10 rst = 0;
    #10 j = 0; k = 0;
    #10 j = 0; k = 1;
    #10 j = 1; k = 0;
    #10 j = 1; k = 1;
    #10 j = 1; k = 1;
    #10 j = 0; k = 0;
    #20 $finish;
  end
  initial begin
    $monitor("Time=%0t | clk=%b | rst=%b | j=%b | k=%b | q=%b", $time, clk, rst, j, k, q);
  end
endmodule
```
#### SIMULATION OUTPUT

<img width="1633" height="669" alt="Screenshot 2025-09-17 092332" src="https://github.com/user-attachments/assets/61643d6a-06f9-41c5-be43-7240993cb93c" />

---
### D Flip-Flop (Blocking)
```verilog
module d_ff(clk,rst,D,Q);
input clk,rst,D;
output reg Q;
always @ (posedge clk)
begin
  if (rst==1)
     Q=0;
  else
     Q=D;
end
endmodule
```
### D Flip-Flop Test bench 
```verilog
module tb_d_ff;
  reg clk,rst,D;
  wire Q;
  d_ff uut(clk,rst,D,Q);
  always #5 clk=~clk;
  initial
  begin
    clk=0;
    D=0;
    rst=1;#10;
    rst=0;
    D=0; #10;
    D=1;
  end
endmodule
```

#### SIMULATION OUTPUT

<img width="1918" height="1079" alt="Screenshot 2025-09-17 205213" src="https://github.com/user-attachments/assets/009999f2-3e8f-499a-a6dc-6e62c9747a16" />


---

### T Flip-Flop (Blocking)
```verilog
module t_ff(clk,rst,t,q);
input clk,rst,t;
output reg q;
always@(posedge clk)
begin
if (rst==1)
q=0;
else if(t==0)
q=q;
else
q = ~q;
end
endmodule
```
### T Flip-Flop Test bench 
```verilog
module tb_t_ff;
reg clk,rst,t;
wire q;
t_ff uut(clk,rst,t,q);
always #5 clk = ~clk;
initial
begin
clk = 0;
rst = 1;
t = 0;
#10 rst = 0;
#10 t = 0;
#10 t = 1;
#20 $finish;
end
initial
begin
$monitor("Time=%0t, clk=%b, rst=%b, t=%b, q=%b", $time, clk, rst, t, q);
end
endmodule
```

#### SIMULATION OUTPUT

<img width="1917" height="1077" alt="Screenshot 2025-09-17 204706" src="https://github.com/user-attachments/assets/770cb99e-c199-4047-981a-af48fc4778aa" />


---

### RESULT

All flip-flops (SR, D, JK, T) were successfully simulated using blocking statements in Verilog HDL.
The outputs matched the expected truth table values, demonstrating correct sequential behavior.

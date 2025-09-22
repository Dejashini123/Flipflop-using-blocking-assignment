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
`timescale 1ns/1ps
module sr_ff (
    input wire S, R, clk,
    output reg Q
);
    always @(posedge clk) begin
        if (S == 0 && R == 0)
            Q = Q;
        else if (S == 0 && R == 1)
            Q = 0;
        else if (S == 1 && R == 0)
            Q = 1;
        else if (S == 1 && R == 1)
            Q = 1'bx;
    end
endmodule
```
### SR Flip-Flop Test bench 
```verilog
module tb_sr_ff;
    reg S, R, clk;
    wire Q;

    sr_ff uut (.S(S), .R(R), .clk(clk), .Q(Q));

    initial begin
        clk = 0;
        forever #5 clk = ~clk;
    end

    initial begin
        S=0; R=0; #10;  
        S=1; R=0; #10;  
        S=0; R=1; #10;  
        S=1; R=1; #10;  
        S=0; R=0; #10;  
        $stop;
    end
endmodule



```
#### SIMULATION OUTPUT

<img width="1918" height="1197" alt="image" src="https://github.com/user-attachments/assets/23dde995-1531-47cc-aaca-6b2a2fb6f3a2" />

---

### JK Flip-Flop (Blocking)
```verilog
`timescale 1ns/1ps
module jk_ff (
    input wire J, K, clk,
    output reg Q
);
    always @(posedge clk) begin
        if (J == 0 && K == 0)
            Q = Q;
        else if (J == 0 && K == 1)
            Q = 0;
        else if (J == 1 && K == 0)
            Q = 1;
        else if (J == 1 && K == 1)
            Q = ~Q;
    end
endmodule
```
### JK Flip-Flop Test bench 
```verilog
module tb_jk_ff;
    reg J, K, clk;
    wire Q;

    jk_ff uut (.J(J), .K(K), .clk(clk), .Q(Q));

    initial begin
        clk = 0;
        forever #5 clk = ~clk;   // clock with 10ns period
    end

    initial begin
        J=0; K=0; #10;   // Hold
        J=1; K=0; #10;   // Set
        J=0; K=1; #10;   // Reset
        J=1; K=1; #10;   // Toggle
        J=0; K=0; #10;   // Hold again
        $stop;
    end
endmodule



```
#### SIMULATION OUTPUT

<img width="1918" height="1197" alt="image" src="https://github.com/user-attachments/assets/3568f3ab-48b2-4127-8f6f-69081c870f23" />

---
### D Flip-Flop (Blocking)
```verilog
`timescale 1ns/1ps
module d_ff (
    input wire D, clk,
    output reg Q
);
    initial Q = 0;  // Initialize Q
    always @(posedge clk) begin
        Q <= D;      // Use non-blocking assignment
    end
endmodule
```
### D Flip-Flop Test bench 
```verilog
module tb_d_ff;
    reg D, clk;
    wire Q;

    d_ff uut (.D(D), .clk(clk), .Q(Q));

    initial begin
        clk = 0;
        forever #5 clk = ~clk;  // Clock with 10ns period
    end

    initial begin
        D = 0; #10;  
        D = 1; #10;  
        D = 0; #10;  
        D = 1; #10;  
        D = 0; #10;  
        $stop;
    end
endmodule



```

#### SIMULATION OUTPUT

<img width="1917" height="1198" alt="image" src="https://github.com/user-attachments/assets/bcd3cc1b-bcb4-4958-8f5a-de73e0a38cd8" />

---
### T Flip-Flop (Blocking)
```verilog
`timescale 1ns/1ps
module t_ff (
    input wire T, clk,
    output reg Q
);
    initial Q = 0;  // Initialize Q
    always @(posedge clk) begin
        Q <= T ? ~Q : Q;
    end
endmodule
```
### T Flip-Flop Test bench 
```verilog
module tb_t_ff;
    reg T, clk;
    wire Q;

    t_ff uut (.T(T), .clk(clk), .Q(Q));

    initial begin
        clk = 0;
        forever #5 clk = ~clk;  // Clock with 10ns period
    end

    initial begin
        T = 0; 
        #2;  // Make sure T is set before the first clock edge
        T = 1; #10;  
        T = 0; #10;  
        T = 1; #10;  
        T = 1; #10;  
        T = 0; #10;  
        $stop;
    end
endmodule



```

#### SIMULATION OUTPUT

<img width="1918" height="1197" alt="image" src="https://github.com/user-attachments/assets/e18e1b9a-6fc6-4960-a096-f20285bc16fe" />


---

### RESULT

All flip-flops (SR, D, JK, T) were successfully simulated using blocking statements in Verilog HDL.
The outputs matched the expected truth table values, demonstrating correct sequential behavior.

# Exp-6a.-Finite-State-Machine-for-Sequence-Detector
# Aim 
To design and simulate a Finite-State-Machine-for-Sequence-Detector-1011 using Verilog HDL, and verify its functionality through a testbench in the Vivado 2023.1 environment. 

# Apparatus Required 

Vivado 2023.1 

# Procedure

*Launch Vivado 2023.1 Open Vivado and create a new project.
*Design the Verilog Code Write the Verilog code for the RAM,ROM,FIFO
*Create the Testbench Write a testbench to simulate the memory behavior. The testbench should apply various and monitor the corresponding output.
*Create the Verilog Files Create both the design module and the testbench in the Vivado project.
*Run Simulation Run the behavioral simulation to verify the output.
*Observe the Waveforms Analyze the output waveforms in the simulation window, and verify that the correct read and write operation.
*Save and Document Results Capture screenshots of the waveform and save the simulation logs. These will be included in the lab report.

# Code
# Mealy Sequence

Verilog code

```verilog
module mealy_sequence(clk, rst, in, out);
    input clk, rst, in;
    output reg out;
    parameter A = 2'b00,
              B = 2'b01,
              C = 2'b10,
              D = 2'b11;
    reg [1:0] current_state, next_state;

    always @(posedge clk or posedge rst) begin
        if (rst)
            current_state <= A;
        else
            current_state <= next_state;
    end

    always @(*) begin
        case (current_state)
            A: next_state = (in == 0) ? A : B;
            B: next_state = (in == 1) ? B : C;
            C: next_state = (in == 0) ? A : D;
            D: next_state = (in == 1) ? B : A;
            default: next_state = A;
        endcase
    end

    always @(*) begin
        out = (current_state == D) ? 1 : 0;
    end
endmodule
```

Test bench

```verilog
module mealy_sequence_tb;

    reg clk, rst, in;
    wire out;
    
    mealy_sequence uut (clk,rst,in,out);
    
    initial  clk = 0;
    always #5 clk = ~clk;   
    initial 
    begin
    in=0;
    rst=0;
    in=1;#10;
    in=0;#10;
    in=1;#10;
    in=0;#10;
    in=0;#10;
    in=1;#10;
    in=0;#10;
    in=1;#10;
    $finish;
    end
endmodule
```


Output Waveform
![WhatsApp Image 2025-10-22 at 13 55 25_dd0d4941](https://github.com/user-attachments/assets/1d360570-87e1-4010-a633-ac105d5dfd73)

# Moore Sequence

Verilog Code

```verilog
module mooresequence(clk, rst, in, out);
    input clk;
    input rst;
    input in;
    output reg out;
parameter S0 = 3'b000,
              S1 = 3'b001,
              S2 = 3'b010,
              S3 = 3'b011,
              S4 = 3'b100;
reg [2:0] current_state, next_state;
always @(posedge clk or posedge rst) begin
        if (rst)
            current_state <= S0;
        else
            current_state <= next_state;
    end
  always @(*) begin
        case (current_state)
            S0: if (in)
                    next_state = S1;
                else
                    next_state = S0;

            S1: begin
                if (in)
                    next_state = S1;
                else
                    next_state = S2;
            end

            S2: begin
                if (in)
                    next_state = S3;
                else
                    next_state = S0;
            end

            S3: begin
                if (in)
                    next_state = S4;
                else
                    next_state = S2;
            end

            S4: begin
                if (in)
                    next_state = S1;
                else
                    next_state = S0;
            end

            default: next_state = S0;
        endcase
    end

    always @(*) begin
        case (current_state)
            S4: out = 1'b1;
            default: out = 1'b0;
        endcase
    end
endmodule
```

Test bench

```verilog
`timescale 1ns/1ps
module tb_mooresequence;
    reg clk, rst, in;
    wire out;

    mooresequence uut (clk, rst, in, out);

    initial begin
        clk = 0;
        forever #5 clk = ~clk;
    end

    initial begin
        rst = 1;
        in = 0;
        #10 rst = 0;

        in = 1; #10;
        in = 0; #10;
        in = 1; #10;
        in = 1; #10;
        
        in = 1; #10;
        in = 0; #10;
        in = 1; #10;
        in = 0; #10;

        #10 $finish;
    end
endmodule
```

Output Waveform
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/084bc89e-93ef-45dc-9240-0481c49d2b42" />


# Conclusion
The Mealy and Moore state machine for sequence 1011 was designed and successfully simulated using Verilog HDL. The testbench verified both the write and read functionalities by simulating the sequence operations and observing the output waveforms.

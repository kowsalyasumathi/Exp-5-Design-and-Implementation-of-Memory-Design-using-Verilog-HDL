# Exp-5-Design-and-Simulate the-Memory-Design-using-Verilog-HDL

#Aim

To design and simulate a RAM,ROM,FIFO using Verilog HDL, and verify its functionality through a testbench in the Vivado 2023.1 environment.
Apparatus Required
Vivado 2023.1
Procedure
1. Launch Vivado 2023.1
Open Vivado and create a new project.
2. Design the Verilog Code
Write the Verilog code for the RAM,ROM,FIFO
3. Create the Testbench
Write a testbench to simulate the memory behavior. The testbench should apply various and monitor the corresponding output.
4. Create the Verilog Files
Create both the design module and the testbench in the Vivado project.
5. Run Simulation
Run the behavioral simulation to verify the output.
6. Observe the Waveforms
Analyze the output waveforms in the simulation window, and verify that the correct read and write operation.
7. Save and Document Results
Capture screenshots of the waveform and save the simulation logs. These will be included in the lab report.

# RAM
# Verilog code
```
module ram_4kb (clk,we,addr,din,dout);
    input clk;
    input we;                  // Write Enable
    input [11:0] addr;         // 12-bit Address = 4096 locations
    input [7:0] din;           // Data input
    output reg [7:0] dout;      // Data output


reg [7:0] mem [0:4095];        // 4KB = 4096 x 8-bit

always @(posedge clk) 
begin
    if (we)
        mem[addr] <= din;      // Write operation
        dout <= mem[addr];         // Read operation
end

endmodule
```
# Test bench
```
module tb_ram_4kb;

reg clk;
reg we;
reg [11:0] addr;
reg [7:0] din;
wire [7:0] dout;

integer i;


ram_4kb dut (clk,we,addr,din,dout);

initial begin
    clk = 0;
    forever  #5 clk = ~clk;
end
initial 
   begin
    we = 0;
    addr = 0;
    din = 0;
    #10;

       for (i = 0; i < 20; i = i + 1) 
        begin
        @(posedge clk);
        addr = $random % 4096;   // Random address (0–4095)
        din  = $random % 256;    // Random 8-bit data (0–255)
        we   = 1;
        @(posedge clk);
        we   = 0;
        end
$finish;
end
endmodule
```

# output Waveform



# ROM
 // write verilog code for ROM using $random
 
 // Test bench

// output Waveform

 # FIFO
 // write verilog code for FIFO
 
 // Test bench

// output Waveform



# Conclusion
The RAM, ROM, FIFO memory with read and write operations was designed and successfully simulated using Verilog HDL. The testbench verified both the write and read functionalities by simulating the memory operations and observing the output waveforms. The experiment demonstrates how to implement memory operations in Verilog, effectively modeling both the reading and writing processes.
 
 


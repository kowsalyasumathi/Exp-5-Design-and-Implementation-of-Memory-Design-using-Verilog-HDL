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

<img width="1920" height="1080" alt="Screenshot (403)" src="https://github.com/user-attachments/assets/17474b17-2c18-4c28-a360-5bc46f8dd52d" />


# ROM
 // write verilog code for ROM using $random
 
 // Test bench

// output Waveform

 # FIFO
 // write verilog code for FIFO
```
module fifo_sync (clk,rst,wr_en,rd_en,data_in,full,data_out,empty,count);
    input clk;
    input rst;
    input wr_en;
    input [7:0] data_in;
    input rd_en;
    output reg full;
    output reg [7:0] data_out;
    output reg empty;
    output reg [4:0] count;

    reg [7:0] mem [0:15];
    reg [3:0] wr_ptr;
    reg [3:0] rd_ptr;
always @(posedge clk) 
   begin
        if (rst) 
          begin
            wr_ptr   <= 0;
            rd_ptr   <= 0;
            count    <= 0;
            data_out <= 0;
            full     <= 0;
            empty    <= 1;
          end 
      else 
        begin
            full  <= (count == 16);
            empty <= (count == 0);
if (wr_en && !full) 
     begin
            mem[wr_ptr] <= data_in;
            wr_ptr <= wr_ptr + 1'b1;
      end

  if (rd_en && !empty) 
      begin
                data_out <= mem[rd_ptr];
                rd_ptr <= rd_ptr + 1'b1;
      end

case ({wr_en && !full, rd_en && !empty})
                2'b10: count <= count + 1'b1;
                2'b01: count <= count - 1'b1;
                default: count <= count;
endcase
 full  <= (count == 16);
            empty <= (count == 0);
        end
    end
endmodule
```
 
 
 // Test bench
 ```
`timescale 1ns/1ps

module fifo_sync_tb;
    reg clk;
    reg rst;
    reg wr_en;
    reg rd_en;
    reg [7:0] data_in;
    wire [7:0] data_out;
    wire full;
    wire empty;
    wire [4:0] count;

   fifo_sync uut (clk,rst,wr_en,rd_en,data_in,full,data_out,empty,count );

 always #5 clk = ~clk;

 initial 
     begin
        clk = 0;
        rst = 1;
        wr_en = 0;
        rd_en = 0;
        data_in = 8'h00;            #10;
        rst = 0;                          #10;
        rst = 1;                          #10;
        rst = 0;                         

  repeat (5) 
     begin
            @(posedge clk);
            wr_en = 1;
            data_in = data_in + 1;
     end
        @(posedge clk);
        wr_en = 0;
 repeat (3) 
          begin
            @(posedge clk);
            rd_en = 1;
        end
        @(posedge clk);
        rd_en = 0;
 #20;
        $finish;
    end

endmodule
```
 

// output Waveform

<img width="1920" height="1080" alt="Screenshot (404)" src="https://github.com/user-attachments/assets/551001af-44b6-4548-ad0f-3d3e6b5569d4" />


# Conclusion
The RAM, ROM, FIFO memory with read and write operations was designed and successfully simulated using Verilog HDL. The testbench verified both the write and read functionalities by simulating the memory operations and observing the output waveforms. The experiment demonstrates how to implement memory operations in Verilog, effectively modeling both the reading and writing processes.
 
 


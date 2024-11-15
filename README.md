**SIMULATION AND IMPLEMENTATION OF LOGIC GATES**

**AIM:**

To design and simulate a 4:1 Multiplexer (MUX) using Verilog HDL in four different modeling styles—Gate-Level, Data Flow, Behavioral, and Structural—and to verify its functionality through a testbench using the Vivado 2023.1 simulation environment. The experiment aims to understand how different abstraction levels in Verilog can be used to describe the same digital logic circuit and analyze their performance.

**APPARATUS REQUIRED:**

Vivado 2023.1

**PROCEDURE:**
1. Launch Vivado
Open Vivado 2023.1 by double-clicking the Vivado icon or searching for it in the Start menu.
2. Create a New Project
Click on "Create Project" from the Vivado Quick Start window.
In the New Project Wizard:
Project Name: Enter a name for the project (e.g., Mux4_to_1).
Project Location: Select the folder where the project will be saved.
Click Next.
Project Type: Select RTL Project, then click Next.
Add Sources:
Click on "Add Files" to add the Verilog files (e.g., mux4_to_1_gate.v, mux4_to_1_dataflow.v, etc.).
Make sure to check the box "Copy sources into project" to avoid any external file dependencies.
Click Next.
Add Constraints: Skip this step by clicking Next (since no constraints are needed for simulation).
Default Part Selection:
You can choose a part based on the FPGA board you are using (if any).
If no board is used, you can choose any part, for example, xc7a35ticsg324-1L (Artix-7).
Click Next, then Finish.
3. Add Verilog Source Files
In the "Sources" window, right-click on "Design Sources" and select Add Sources if you didn't add all files earlier.
Add the Verilog files (mux4_to_1_gate.v, mux4_to_1_dataflow.v, etc.) and the testbench (mux4_to_1_tb.v).
4. Check Syntax
Expand the "Flow Navigator" on the left side of the Vivado interface.
Under "Synthesis", click "Run Synthesis".
Vivado will check your design for syntax errors. If any errors or warnings appear, correct them in the respective Verilog files and re-run the synthesis.
5. Simulate the Design
In the Flow Navigator, under "Simulation", click on "Run Simulation" → "Run Behavioral Simulation".
Vivado will open the Simulations Window, and the waveform window will show the signals defined in the testbench.
6. View and Analyze Simulation Results
The simulation waveform window will display the signals (S1, S0, A, B, C, D, Y_gate, Y_dataflow, Y_behavioral, Y_structural).
Use the time markers to verify the correctness of the 4:1 MUX output for each set of inputs.
You can zoom in/out or scroll through the simulation time using the waveform viewer controls.
7. Adjust Simulation Time
To run a longer simulation or adjust timing, go to the Simulation Settings by clicking "Simulation" → "Simulation Settings".
Under "Simulation", modify the Run Time (e.g., set to 1000ns).
8. Generate Simulation Report
Once the simulation is complete, you can generate a simulation report by right-clicking on the simulation results window and selecting "Export Simulation Results".
Save the report for reference in your lab records.
9. Save and Document Results
Save your project by clicking File → Save Project.
Take screenshots of the waveform window and include them in your lab report to document your results.
You can include the timing diagram from the simulation window showing the correct functionality of the 4:1 MUX across different select inputs and data inputs.
10. Close the Simulation
Once done, close the simulation by going to Simulation → "Close Simulation".

**Logic Diagram:**
![image](https://github.com/user-attachments/assets/d4ab4bc3-12b0-44dc-8edb-9d586d8ba856)

**Truth Table:**
![image](https://github.com/user-attachments/assets/c850506c-3f6e-4d6b-8574-939a914b2a5f)

**Verilog Code:**
**4:1 MUX Gate-Level Implementation**
~~~
module mux4_to_1_gate (a,b,c,d,s,y);
input a,b,c,d;
output y;
wire [4:1]w;
and g1(w[1],~s[1],~s[0],a);
and g2(w[2],~s[1],s[0],b); and g3(w[3],s[1],~s[0],c);
and g4(w[4],s[1],s[0],d); or g5(y,w[1],w[2],w[3],w[4]);
~~~

**Output:**
![1](https://github.com/user-attachments/assets/c5d42a9d-2829-401f-a2d2-86740ea1efdc)


**4:1 MUX Data Flow Implementation**
~~~
module dataflow(a,b,c,d,s1,s0,y);
input a,b,c,d,s1,s0;
output y;
assign y = (~s1 & ~s0 & a) | (~s1 & s0 & b) | (s1 & ~s0 & c) | (s1 & s0 & d);
endmodule
~~~

**Output:**
![2](https://github.com/user-attachments/assets/055e61d4-215f-4475-9c27-a04460c23eec)


**4:1 MUX Behavioral Implementation:**
~~~
module behavior(A,B,C,D,S1,S0,Y);
input A,B,C,D,S1,S0;
output reg Y;
always @(*)
begin
case ({S1, S0})
                2'b00: Y = A;
                2'b01: Y = B;
                2'b10: Y = C;
                2'b11: Y = D;
endcase
end
endmodule
~~~

**Output:**
![beh](https://github.com/user-attachments/assets/213bb30e-8d41-4a53-a8f3-b3cbe5a36bd7)


**4:1 MUX Structural Implementation:**
~~~
module mux_4to1 (a,b,c,d,S0,S1,Y);
input a,b,c,d;
input S0, S1;
output Y ;
assign Y = (S1 == 0 && S0 == 0) ? a :(S1 == 0 && S0 == 1) ? b :(S1 == 1 && S0 == 0) ? c :(S1 == 1 && S0 == 1) ? d;
endmodule
~~~

**Output:**
![beh](https://github.com/user-attachments/assets/213bb30e-8d41-4a53-a8f3-b3cbe5a36bd7)


**Testbench Implementation:**

~~~
module multiplexer_tb;
 // Declare inputs as reg and outputs as wire
 reg s1, s0, a, b, c, d;
 wire y;

 // Instantiate the multiplexer module
 multiplexer uut (
   .s1(s1), 
   .s0(s0), 
   .a(a), 
   .b(b), 
   .c(c), 
   .d(d), 
   .y(y)
 );

 // Test cases
 initial begin
   // Monitor changes in inputs and output
   $monitor("s1 = %b, s0 = %b, a = %b, b = %b, c = %b, d = %b, y = %b", s1, s0, a, b, c, d, y);
   
   // Apply test vectors
   s1 = 0; s0 = 0; a = 1; b = 0; c = 0; d = 0; #10;  // Test case 1
   s1 = 0; s0 = 1; a = 0; b = 1; c = 0; d = 0; #10;  // Test case 2
   s1 = 1; s0 = 0; a = 0; b = 0; c = 1; d = 0; #10;  // Test case 3
   s1 = 1; s0 = 1; a = 0; b = 0; c = 0; d = 1; #10;  // Test case 4
   
   // Finish simulation
   $finish;
 end
endmodule
~~~

**Output:**
![image](https://github.com/user-attachments/assets/ebb83ef3-3844-4f9a-9809-ae9a681feca6)

**Conclusion:**

In this experiment, a 4:1 Multiplexer was successfully designed and simulated using Verilog HDL across four different modeling styles: Gate-Level, Data Flow, Behavioral, and Structural. The simulation results verified the correct functionality of the MUX, with all implementations producing identical outputs for the given input conditions.



  

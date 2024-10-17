# TRAFFIC-LIGHT-CONTROLLER-USING-VERILOG-HDL

## AIM:

To design and simulate a traffic light controller using Verilog HDL, and verify its functionality through a testbench in the Vivado 2023.1 simulation environment. The objective is to control the traffic lights for a junction with a specific time-based sequence for Red, Yellow, and Green lights.

## APPARATUS REQUIRED:

Vivado 2023.1 or equivalent Verilog simulation tool.
Computer system with a suitable operating system.
FPGA board (optional for hardware verification).

## PROCEDURE:

Launch Vivado 2023.1:

Open Vivado and create a new project.
Design the Traffic Light Controller Verilog Code:

Write the Verilog code for the traffic light controller, using an FSM (Finite State Machine) to transition between Green, Yellow, and Red lights based on timing intervals.
Create the Testbench:

Write a testbench to simulate the traffic light controller. The testbench will check the sequence of light transitions based on time.
Add the Verilog Files:

Add the traffic light controller Verilog code and the testbench file to the project.
Run Simulation:

Run the behavioral simulation in Vivado to verify the correct sequence of the traffic lights.
Observe the Waveforms:

Examine the waveform output to verify that the traffic light transitions through the Green, Yellow, and Red lights in the correct sequence.
Save and Document Results:

Capture screenshots of the waveform and save the simulation logs to include in your report.

## VERILOG CODE FOR TRAFFIC LIGHT CONTROLLER:

~~~

module traffic_light_controller (
    input wire clk,         // Clock signal
    input wire rst,         // Reset signal
    output reg [2:0] lights // 3-bit output for lights: [Green, Yellow, Red]
);

    // State encoding
    parameter GREEN  = 3'b001; // Green light
    parameter YELLOW = 3'b010; // Yellow light
    parameter RED    = 3'b100; // Red light

    reg [1:0] state;           // Current state
    reg [3:0] timer;           // Timer for light duration

    // State transition logic
    always @ (posedge clk or posedge rst) begin
        if (rst) begin
            state <= GREEN;    // Start with green light
            timer <= 4'b0000;  // Reset timer
            lights <= GREEN;    // Output green light
        end else begin
            if (timer == 4'b1111) begin // Timer reached maximum
                case (state)
                    GREEN: begin
                        state <= YELLOW; // Transition to yellow
                        lights <= YELLOW; // Output yellow light
                    end
                    YELLOW: begin
                        state <= RED; // Transition to red
                        lights <= RED; // Output red light
                    end
                    RED: begin
                        state <= GREEN; // Transition to green
                        lights <= GREEN; // Output green light
                    end
                endcase
                timer <= 4'b0000; // Reset timer
            end else begin
                timer <= timer + 1; // Increment timer
            end
        end
    end
endmodule


~~~

## OUTPUT:

![image](https://github.com/user-attachments/assets/741ce599-f40b-43ad-83ee-5eed6cc06a3e)

## TESTBENCH FOR TRAFFIC LIGHT CONTROLLER:

~~~

// traffic_light_controller_tb.v
`timescale 1ns / 1ps

module traffic_light_controller_tb;

    // Inputs
    reg clk;
    reg reset;

    // Outputs
    wire [2:0] lights;

    // Instantiate the Unit Under Test (UUT)
    traffic_light_controller uut (
        .clk(clk),
        .reset(reset),
        .lights(lights)
    );

    // Clock generation
    always #5 clk = ~clk;  // Toggle clock every 5 ns

    // Test procedure
    initial begin
        // Initialize inputs
        clk = 0;
        reset = 1;

        // Release reset after some time
        #10 reset = 0;

        // Run simulation for 100 ns to observe light transitions
        #100 $stop;
    end

    // Monitor outputs
    initial begin
        $monitor("Time=%0t | Lights (R Y G) = %b", $time, lights);
    end

endmodule
~~~

## OUTPUT:

![image](https://github.com/user-attachments/assets/27d3e3ff-8508-48c8-becf-e55527448ba2)


## CONCLUSION:

In this experiment, a traffic light controller was successfully designed and simulated using Verilog HDL. The design controlled the traffic lights to switch between Green, Yellow, and Red in a cyclic manner based on timing intervals. The testbench verified that the traffic lights followed the correct sequence and timing. The simulation results confirm the correct functionality of the traffic light controller, demonstrating the effectiveness of Verilog HDL in designing FSM-based controllers for real-world applications.

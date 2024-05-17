# ELEVATOR_CONTROL
# AIM
 To stimulate and synthesis ELEVATOR_CONTROL using Vivado

# Software Required:
vivado 2023.2 software.

# Procedure:

STEP:1 Start the vivado software, Select and Name the New project

STEP:2 Select the device family, device, package and speed.

STEP:3 Select new source in the New Project and select Verilog Module as the Source type.

STEP:4 Type the File Name and module name and Click Next and then finish button. Type the code and save it.

STEP:5 Select the run simulation and then run Behavioral Simulation in the Source Window and click the check syntax.

STEP:6 Click the simulation to simulate the program and give the inputs and verify the outputs as per the truth table.

STEP:7 compare the output with truth table. image

# STATE DIAGRAM
![image](https://github.com/RESMIRNAIR/ELEVATOR_CONTROL/assets/154305926/b42a1942-752f-4787-967c-b9c13ab3e763)

#Program
module Lift8(clk, reset, req_floor, idle, door, Up, Down, current_floor, requests, max_request, min_request, emergency_stop);

input clk, reset, emergency_stop;

input logic [2:0] req_floor; // 3-bit input for 8 floors (0 to 7)

output logic [1:0] door;

output logic [2:0] max_request;

output logic [2:0] min_request;

output logic [1:0] Up;

output logic [1:0] Down;

output logic [1:0] idle;

output logic [2:0] current_floor;

output logic [7:0] requests;

logic door_timer;

logic emergency_stopped;

logic flag=0;

// Update requests when a new floor is requested

always @(req_floor)

begin

requests[req_floor] = 1; // Update max_request and min_request based on requested floors

if (max_request < req_floor)

begin

max_request = req_floor;

end

if (min_request > req_floor)

begin

min_request = req_floor;

end

// Update max_request and min_request based on current floor

if (requests[max_request] == 0 && req_floor > current_floor)

begin

max_request = req_floor;

end

if (requests[min_request] == 0 && req_floor < current_floor)

begin

min_request = req_floor;

end end

// Check and update lift behavior based on current floor

always @(current_floor )

begin

if (requests[current_floor] == 1)

begin

idle = 1;

door = 1;

requests[current_floor] = 0;

door_timer = 1; // Start the door timer when opening

end end

// State machine for lift control

always @(posedge clk )

begin

if (door_timer == 1)

begin

door <= 0; // Close the door after the one clock expires

//$display("%h", current_floor);

end

if (reset)

begin

// Reset lift to initial state

flag=0;

current_floor <= 0;

idle <= 0;

door <= 0; // door open

Up <= 1; // going up

Down <= 0; // not going down

max_request <= 0;

min_request <= 7;

requests <= 0;

emergency_stopped <= 0; // Initialize emergency stop state

end

else if (requests == 0 && !reset)

begin

// Stay on the current floor if no requests

current_floor <= current_floor;

emergency_stopped <= 0; // Clear emergency stop when not moving

end

// emergency

else if (emergency_stop)

begin

// Emergency stop button is turned on

idle <= 1;

flag <=1;

emergency_stopped <= 1; // Set emergency stop state

end

else if (emergency_stopped && emergency_stop)

begin

// Remain stopped until the emergency stop button is reset

current_floor <= current_floor;

door <= 0; // Keep the door closed during an emergency stop

end

// emergency reset

else if (!emergency_stop && flag)

begin

// Emergency stop button is turned off

emergency_stopped <= 0; // Set emergency stop state

flag <=0;

end

else

begin

// Normal operation when not in emergency stop

if (max_request <= 7)

begin

if (min_request < current_floor && Down == 1)

begin

  // Move down one floor
  
  current_floor <= current_floor - 1;
  
  door <= 0;
  
  idle <= 0;
  
end

else if (max_request > current_floor && Up == 1)

begin

  // Move up one floor
  
  current_floor <= current_floor + 1;
  
  door <= 0;
  
  idle <= 0;
  
end

else if (req_floor == current_floor)

begin

     // Open door and handle request
     
    door <= 1;
    
    idle <= 1;
    
end

else if (max_request == current_floor)

begin

  Up <= 0;
  
  Down <= 1;
  
end

else if (min_request == current_floor)

begin

  Up <= 1;
  
  Down <= 0;
  
end
end

end end

endmodule
# OUTPUT
![301737742-6379ab0d-b73f-4740-bdda-45263b28f120](https://github.com/RESMIRNAIR/ELEVATOR_CONTROL/assets/160302888/d036af0e-67d9-40f8-b2fa-a4b26ed8ac66)
# Result
Thus the verilog program for ELEVATOR_CONTROL has been simulated and verified successfully.

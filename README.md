# Lab 2
module Lab2_Truex (in, clock, reset, out);
input clock, reset, in;
output reg out; 

//define states
parameter state0 = 4'b0000,
state1 = 4'b0001,
state2 = 4'b0010,
state3 = 4'b0100,
state4 = 4'b1001;
reg [3:0] current, next;

//reset the fsm if reset is toggled
always @(posedge clock, posedge reset)
begin
	if(reset == 1)
	current <= state0;
	else
	current <= next;
end

//transitional logic
always @(current, in)
begin
	case (current)
	state0:begin
		if(in == 1)
		next = state1;
		else
		next = state0;
	end
	state1:begin
		if(in == 0)
		next = state2;
		else
		next = state1;
	end
	state2:begin
		if(in == 0)
		next = state3;
		else
		next = state2;
	end
	state3:begin
		if(in == 1)
		next = state4;
		else
		next = state3;
	end
	state4:begin
		if(in == 1)
		next = state0;
		else
		next = state2;
	end
	default: next = state0;
	endcase
end

//retrieve output based on current state of fsm
always @(current)
begin
	case(current)
	state0: out = 0;
	state1: out = 0;
	state2: out = 0;
	state3: out = 0;
	state4: out = 1;
	default: out = 0;
	endcase
end 
endmodule

module Lab2_Truex_tb;
reg in;
reg clock;
reg reset;
wire out;

Lab2_Truex dut(.in(in), .clock(clock), .reset(reset), .out(out));
initial begin
clock = 0;
forever #5 clock = ~clock;
end

initial begin
	in = 0;
	reset = 1;
	#50;
	reset = 0;
	#50;
	in = 1;
	#50;
	in = 0;
	#50;
	in = 0;
	#50;
	in = 1;
	#50;
	in = 0;
	#50;
	in = 0;
	#50;
	in = 1;
	#50;
	in = 1;
	#50;
	in = 0;
	#50;
	in = 0;
	#50;
	in = 1;
	#50;
	in = 0;
	#50;
	reset = 1;
	#50;
	reset = 0;
end
endmodule 

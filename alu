// this represents the alu unit

module alu (RESULT_o,
flag_carry,
flag_zero, flag_overflow, flag_negative,
A_i, B_i, ALU_CONTROL_i); 

// defining inputs
// A&B = 32_bits while ALU_CONTROL = 3 bits
input [31:0]A_i,B_i;
input [2:0]ALU_CONTROL_i;



// defining outputs
// RESULT = 32bits while carry_out = 1bit
output [31:0]RESULT_o;
output flag_carry;
output flag_negative;
output flag_overflow;
output flag_zero;
  
// defining wires
wire [31:0]out_or, out_and;
wire [31:0]out_not;
wire [31:0]out_mux;
wire [31:0]out_adder;
wire [31:0]out_sign_extension;
wire carry_out;

 // defining reg
reg [31:0]RESULT_o;

// using blocking assignment and data flow modelling 
assign out_or    = A_i | B_i;
assign out_and   = A_i & B_i;
assign out_not   = ~B_i;
assign {carry_out,out_adder} = A_i + out_mux + ALU_CONTROL_i[0];
//assign carry_out = (A_i&out_mux) + (out_mux&ALU_CONTROL_i[0]) + (ALU_CONTROL_i[0]&A_i);

//using concatenation for sign extension 
assign out_sign_extension = {31'd0,out_adder[31]};

// initializing mux using ternary operator
assign out_mux = (ALU_CONTROL_i[0] == 0) ? B_i : out_not ;

  // initializing mux using case statement
always @(*) begin
                case (ALU_CONTROL_i[2:0])
                        3'b010 : RESULT_o = out_and;
                        3'b011 : RESULT_o = out_or;
                        3'b101 : RESULT_o = out_sign_extension;
                        default : RESULT_o = out_adder; 
               endcase
            end
  // flag logic
assign flag_carry = carry_out & (ALU_CONTROL_i[1]);
  assign flag_negative = ~(ALU_CONTROL_i[1]^A_i[31]^B_i[31]^out_adder[31]);
assign flag_zero = &(~RESULT_o);
  assign flag_overflow = ~(ALU_CONTROL_i[1]^A_i[31]^B_i[31]^out_adder[31]);

endmodule

```verilog
module univ_shift_reg4_sync_rst_n( input wire [3:0] p_din,
                                 input wire clk, rst_n, s_right_din, s_left_din,
                                 input wire [1:0] select,
                                 output reg [3:0] p_dout,
                                 output wire s_right_dout, s_left_dout);
  
  always @ (posedge clk) begin
    if(rst_n == 0) 
      p_dout <= 0;
    else case (select)
      2'b01: p_dout <= {s_right_din, p_dout[3:1]};
      2'b10: p_dout <= {p_dout[2:0], s_left_din};
      2'b11: p_dout <= p_din;
      default: p_dout <= p_dout;
    endcase
  end
  assign s_right_dout = p_dout[3];
  assign s_left_dout = p_dout[0];
endmodule
    
                                 

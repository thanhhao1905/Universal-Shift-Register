```verilog
`timescale 1ps/1ps
module tb_univ_shift_reg4_sync_rst_n;
  reg [3:0] p_din;
  reg clk, rst_n, s_right_din ,s_left_din;
  reg [1:0] select;
  wire [3:0] p_dout;
  reg [3:0] exp_p_dout;
  wire s_right_dout ,s_left_dout;
  integer err =0;
  
  univ_shift_reg4_sync_rst_n DUT ( p_din ,clk ,rst_n ,s_right_din ,s_left_din ,select ,p_dout, s_right_dout, s_left_dout);
  
  always #2 clk = ~clk;
  initial begin
    $monitor("time=%0t ,rst_n =%b, clk =%b, p_din=%b ,s_right_din=%b ,s_left_din=%b ,select=%b --> p_dout=%b | exp_p_dout =%b, s_right_dout=%b | s_left_dout=%b",$time , rst_n, clk, p_din , s_right_din, s_left_din, select ,p_dout,exp_p_dout,s_right_dout,s_left_dout);
    
    rst_n = 0;
    clk = 0;
    p_din = 0;
    s_right_din = 0;
    s_left_din = 0;
    select = 0;
    exp_p_dout =0;
    
    #1 rst_n =1;
    p_din = 4'b1011 ;
    s_right_din = 1;
    s_left_din = 1;
    select = 2'b11;
    #4;
    select = 2'b01;
    #2;
    select = 2'b10;
    #4;
    exp_p_dout = 4'b1011;
    select = 2'b00;
    #1;
    check(p_dout,exp_p_dout);
    
    #10;
    
    p_din = 4'b1001 ;
    s_right_din = 0;
    s_left_din = 0;
    select = 2'b11;
    #3;
    select = 2'b10;
    #3;
    select = 2'b01;
    #4;
    select = 2'b00;
    exp_p_dout = 4'b0001;
    #1;
    check(p_dout,exp_p_dout);
    
    
    #10;
    
    if(err ==0)begin
      $display("----------");
      $display("Test Pass");
      $display("----------");
    end else begin
      $display("----------");
      $display("Test Fail with %d error",err);
      $display("----------");
    end
    
    $finish;
  end
  
  task check(input [3:0]p_dout ,input [3:0]exp_p_dout);
    begin
      @( posedge clk);
      #1;
      if(p_dout !== exp_p_dout) begin
        $display("[CHECK] Error");
        err = err +1;
      end else begin
        $display("[CHECK] Matching");
      end
    end
  endtask
  
  initial begin
    $dumpfile("dump.vcd");
    $dumpvars;
  end
endmodule

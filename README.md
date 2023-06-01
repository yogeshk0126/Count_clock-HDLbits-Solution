# Count_clock-HDLbits-Solution
Here is the verilog code of digital clock which is one of counter problem of HDLbits.  /hdlbits.01xz.net/wiki/Count_clock
/////
module top_module(
    input clk,
    input reset,
    input ena,
    output pm,
    output [7:0] hh,
    output [7:0] mm,
    output [7:0] ss); 
    always @(posedge clk)
 begin
       /////   for ss ////// This block will create second count
        if (reset) ss <=8'd0 ; else if (ena) begin
        if (ss[3:0]==9) ss[3:0]<=0; else ss[3:0]<=ss[3:0] + 1'd1;
            if (ss[3:0]==9 && ss[7:4]==5) ss[7:4]<=0; else if (ss[3:0]==9) ss[7:4] <= ss[7:4]+1'd1;
        end
        ///// for mm //// This block with create minute count
        if (reset) mm <=8'd0 ; else if (ena) begin
            if (ss[3:0]==9 && ss[7:4]==5 && mm[3:0]==9) mm[3:0]<=0; 
            else if(ss[3:0]==9 && ss[7:4]==5) mm[3:0]<=mm[3:0] + 1'd1;
            if (ss[3:0]==9 && ss[7:4]==5 && mm[3:0]==9 && mm[7:4]==5) mm [7:4]<=0; 
            else if (ss[3:0]==9 && ss[7:4]==5 && mm[3:0]==9 ) mm[7:4] <= mm[7:4]+1'd1;
        end
            /// for hh //// This block with create hour Count
            if (reset) {hh[7:4],hh[3:0]} <= {4'd1 ,4'd2} ; else if (ena) begin
                if (ss[3:0]==9 && ss[7:4]==5 && mm[3:0]==9 && mm[7:4]==5 && hh[3:0]==9 ) hh[3:0]<=0; 
            else if (ss[3:0]==9 && ss[7:4]==5 && mm[3:0]==9 && mm[7:4]==5 ) hh[3:0] <= hh[3:0]+1'd1;
                if(ss[3:0]==9 && ss[7:4]==5 && mm[3:0]==9 && mm[7:4]==5 && hh[3:0]==2 && hh[7:4]==1) {hh[7:4],hh[3:0]}<= {4'd0,4'd1}; ///// here when hh=12 next value of hh=01.
            else if (ss[3:0]==9 && ss[7:4]==5 && mm[3:0]==9 && mm[7:4]==5 && hh[3:0]==9 ) hh[7:4]<= hh[7:4]+1'd1;
        end
        //// This block will be use to create AM/PM .
        if (reset) pm <= 0;
        else if(ss[3:0]==9 && ss[7:4]==5 && mm[3:0]==9 && mm[7:4]==5 && hh[7:4]==1 && hh[3:0]==1) pm<=~pm;     
 end  
                  
endmodule

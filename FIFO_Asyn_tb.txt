`timescale 1ns/1ps

module fifo_asyn_tb1;

parameter DATA_WIDTH = 8;
parameter ADDR_WIDTH = 3;

reg wclk;
reg wrst_n;
reg w_en;

reg rclk;
reg rrst_n;
reg r_en;

reg  [DATA_WIDTH-1:0] data_in;
wire [DATA_WIDTH-1:0] data_out;

wire full;
wire empty;

logic [DATA_WIDTH-1:0] exp_q[$];
logic [DATA_WIDTH-1:0] expected;

//----------------------------------------------------
// DUT
//----------------------------------------------------

asynchronous_fifo
#(
    .DATA_WIDTH(DATA_WIDTH),
    .ADDR_WIDTH(ADDR_WIDTH)
)
DUT
(
    .wclk(wclk),
    .wrst_n(wrst_n),
    .w_en(w_en),

    .rclk(rclk),
    .rrst_n(rrst_n),
    .r_en(r_en),

    .data_in(data_in),
    .data_out(data_out),

    .full(full),
    .empty(empty)
);

//----------------------------------------------------
// Clock Generation
//----------------------------------------------------

initial wclk = 0;
always #10 wclk = ~wclk;

initial rclk = 0;
always #35 rclk = ~rclk;

//----------------------------------------------------
// Reset
//----------------------------------------------------

initial
begin
    wrst_n = 0;
    rrst_n = 0;

    w_en = 0;
    r_en = 0;

    data_in = 0;

    #100;

    wrst_n = 1;
    rrst_n = 1;
end

//----------------------------------------------------
// Write Process
//----------------------------------------------------

integer i;

initial
begin

    wait(wrst_n);

    repeat(2) @(posedge wclk);

    for(i=0;i<20;i=i+1)
    begin

        @(negedge wclk);

        if(!full)
        begin
            w_en = 1;

            data_in = $random;

            exp_q.push_back(data_in);

            $display("WRITE : time=%0t data=%h",
                      $time,data_in);
        end

        @(posedge wclk);

        w_en = 0;

    end

end

//----------------------------------------------------
// Read Process
//----------------------------------------------------

initial
begin

    wait(rrst_n);

    repeat(6) @(posedge rclk);

    forever
    begin

        @(negedge rclk);

        if(!empty && exp_q.size()!=0)
        begin

            r_en = 1;

        end

        @(posedge rclk);

        if(r_en)
        begin

            #1;

            expected = exp_q.pop_front();

            if(data_out!==expected)
            begin
                $display("FAIL  time=%0t expected=%h received=%h",
                         $time,expected,data_out);
            end
            else
            begin
                $display("PASS  time=%0t data=%h",
                         $time,data_out);
            end

        end

        r_en = 0;

    end

end

//----------------------------------------------------
// Finish
//----------------------------------------------------

initial
begin
    #5000;
    $finish;
end

endmodule
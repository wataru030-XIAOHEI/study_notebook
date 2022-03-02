# CHISEL RECORD

## FUNCTION AND TYPE

`val register = RegInit(<rst_value><data_width>):`

​	create a `reg`with rst ,the generated verilog codes like this :

~~~verilog
 reg [11:0] register; 
  wire [11:0] _T_1 = io_in + 12'h1; 
  assign io_out = register; 
  always @(posedge clock) begin
    if (reset) begin 
      register <= 12'h0;
    end else begin
      register <= _T_1; 
    end
  end
~~~

`val register = RegNext(<data_width>)` and 

`val register = Reg(<data_width>)`

create a `reg` without rst in the always block,their generated verilog codes like this  :

~~~verilog
 reg [11:0] register; 
  assign io_out = register; 
  always @(posedge clock) begin
      register <= io_in + 12'h1; 
  end
~~~

`when elsewhen otherwise`like `if else`



`val reg_n : UInt = Reg(UInt(4.W))`

create a `reg` without `rst`,and this `reg` could do things normally do whith `UInt`s,like `+,-`,ext.

`UInt`could change to `SInt`,means signed . like this:

~~~veriog
$(signed)reg_n 
~~~

 

## EXAMPLE

### LFSR

~~~scala
class LFSR (val init:Int = 1 ) extends Module{
    val io = IO(new Bundle{
        val in = Input(UInt(4.W))
        val OT = Output(UInt(4.W))
    })
    val state = RegInit(UInt(4.W),init.U)
    val n_sta = (state << 1) | io.in
    state := n_sta
    io.ot := state
}
~~~


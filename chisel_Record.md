# CHISEL RECORD

## SCALA PART

understanding scala lang .here have something normoally used .

### VAR AND VAL

`var Name = <value>` : easy change __variable__

`val Name = <value> `: const value

### DEF

`def something : something`:define something ,for example :

~~~scala
def times(x:Int):Int = 2 * x //define the function 

//use the defined function
times(2)
~~~

`val const_val = val`:make the const value.

### LIST

`val list_name = List(v1,v2,v3,...)`:build the list(array),vx must be number .

`val list_name = x :: y :: z:: ... ::Nil`: build the list by x,y,z..etc. for example:

~~~scala
val x = 1 
val y = 2
val yy = 3
val list1 = List(1,2,3)
val list2 = x::y::yy::Nil 
//list1 === list2 
~~~

`val name = list_name.length`: length of this list.

`val name = list_name.size`: size of this list.

`val name = list_name.head`:get the first element of this list

`val name = list_name.tail`: get a new list with the first element removed .

`val name = list_name(index)`: get the element of this list(0-indexed).

### FOR STATEMENT

`for(index <- <num1> to <num2> ){}`:included num2

`for(index <- <num1> until <num2>){}`: num2 is not included

`for(index <- <num1> to/until <num2> by <step>){}`:by step for .

### PACKAGES AND IMPORTS

~~~scala
package package_name
class class_name {...}
~~~

externally referencing code defined in a file containing the above lines,should use:

~~~scala
import package_name.class_name
~~~

### A CLASS EXAMPLE

~~~scala
//from https://hub.gke2.mybinder.org/user/freechipsproject-chisel-bootcamp-h2pgr23e/lab/tree/1_intro_to_scala.ipynb


// WrapCounter counts up to a max value based on a bit size
class WrapCounter(counterBits: Int) {

  val max: Long = (1 << counterBits) - 1
  var counter = 0L
    
  def inc(): Long = {
    counter = counter + 1
    if (counter > max) {
        counter = 0
    }
    counter
  }
  println(s"counter created with max value $max")
}

~~~



---

## FUNCTION AND TYPE

* `val register = RegInit(<rst_value><data_width>):`

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

* `val register = RegNext(<data_width>)` and 

* `val register = Reg(<data_width>)`

create a `reg` without rst in the always block,their generated verilog codes like this  :

~~~verilog
 reg [11:0] register; 
  assign io_out = register; 
  always @(posedge clock) begin
      register <= io_in + 12'h1; 
  end
~~~

`when elsewhen otherwise`like `if else`



* `val reg_n : UInt = Reg(UInt(4.W))`

create a `reg` without `rst`,and this `reg` could do things normally do whith `UInt`s,like `+,-`,ext.

`UInt`could change to `SInt`,means signed . like this:

~~~veriog
$(signed)reg_n 
~~~

 ## TESTBENCH

~~~scala
//we can write the testbench to verificate our design .
//necessary imports
import chisel3._
import chiseltest._ //test package
import org.scalatest.flatspec.AnyFlatSpec

//creat a test class

class ModuleTest extends AnyFlatSpec with ChiselSc alatestTester{
    behavior of "Our module"
    //test class body 
    
    test(new module_name){ c =>
        //test body like here's
        c.io.in.poke(0.U)
        c.clock.step()
        c.io.out.expect(0.U) //assert
        c.io.in.poke(42.U)
        c.clock.step()
        c.io.out.expect(42.U)
        println("Last output value :" + c.io.out.peek().litValue)
    }
}
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

## NORMAL IMPORTS

there are normal using imports.

~~~scala
import chisel3._
import chisel3.util._
import chisel3.tester._
import chisel3.tester.RawTester.test
import dotvisualizer._
~~~


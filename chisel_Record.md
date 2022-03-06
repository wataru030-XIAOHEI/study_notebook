# CHISEL RECORD

this markdown notebook is my study record.to write down Chisel notebook. If had something wrong,plz email or push your issue .

@author: wataru030 

@EMAIL: 3119002373@mail2.gdut.edu.cn



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

## CHISEL PART

chisel provides three data types to describe our signals,combination logic,and register.there are:

~~~scala 
//(n.W) --> width is n .

Bits(n.W)//bit
UInt(n.W) //unsigned
SInt(n.W) //signed

//for example
unsigned 8 with 4 width:
8.U(4.W) 
signed -3 with 4 width :
-3.S(4.w)

1.U(32) //--> means catch the 32th bit of 1.U ,result is 0
~~~

### hex,oct,bin :

b:binary

h:hex

o:oct

default : dec

~~~scala
//for example
"hff".U(8.W) //--> 8'hff
"0377".U(21.W)//--->31'o377
"b0011_0100".U(5.W)//--->5'b0011_0100,underscores is ignored

~~~

### Bool

~~~scala
Bool()
true.B 	//true
false.B	//false
~~~

### Combination logic 

~~~scala
and : &
nor : ~
xor: ^
or : |
add: +
sub: -
modulo: %
neg: -<x>
div: /
mul: *
~~~

~~~scala
val w = Wire(UInt()) //define w is wire type
w := a&b //assign w = a& b 
-----
//this is like :
val w = a & b //wire w = a & b 
~~~

* data from high to low

`val w = Word(7,0)`:`//assign w = Word[7:0]`

* Cat

`val w = Cat(high,low)`:`//wire w = {high,low} `

### Mux

`val = mul_result = Mux(sel,a,b)`: when sel is true ,sel a, else sel b. ,a ,b could be a bit or victor

### Counter

~~~scala
val cntReg = RegInit(0.U(8.W))
cntReg := Mux(cntReg === 100.U , 0.U , cntReg + 1.U)
~~~

### Bundle and Vec

a Chisel bundle groups several signals.

~~~scala
val ch = Wire(new Channel())
ch.data := 123.U
ch.valid := true.B

val b = ch.valid

val channel = ch 
~~~

~~~scala
v(0)=1.U
v(1)= 3.U
v(2)=5.U

val index = 1.U(2.W)
val a = v(index) //a = v(1) = 3
~~~

### Regfile

~~~scala

val regFile = Reg(Vec(32,UInt(32.W)))//reg [31:0]regFile[31:0] 

regFile := dataIn
val dataOut = regFile(index)
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

* `val register = RegNext(<data_width>)` and `val reg = RegNext(<data>,<rst_value>)`

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


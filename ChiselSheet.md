## Small table

| Scala (Compile-time) | Chisel (Hardware/Run-time) |
|----------------------|----------------------------|
| Int/BigInt           | UInt, SInt                 |
| Boolean              | Bool                       |
| Seq/Array/Vector     | Vec                        |

## Big table

Notation/explanations

* Compile-time construct (Scala). Does not appear directly in generated hardware. Used during the process of constructing hardware (e.g. parameters).
* Hardware construct (Chisel). Appears in generated hardware in some form.
* `[c]` means to replace that with a value of your choosing.

|            | Scala/Chisel                          | Generic form                           | Compile-time | Hardware / Run-time | Verilog                                                                                                              | Notes                                                                                           |
|------------|---------------------------------------|----------------------------------------|--------------|---------------------|----------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------|
| Constructs | Int, BigInt                           | `val x: Int = [c]`                     | Yes          |                     | `localparam x = [c];`                                                                                                |                                                                                                 |
|            | UInt, SInt                            | `[x].U([w].W)`                         |              | Yes                 | `[w]'d[x]`                                                                                                           |                                                                                                 |
| Examples   | `15.U(4.W)`                           |                                        |              | Yes                 | `4'd15`                                                                                                              |                                                                                                 |
|            | `val param: Int = 16`                 |                                        | Yes          |                     | `localparam param = 16;`                                                                                             |                                                                                                 |
| Constructs | Boolean                               | `val x: Boolean = [b]`                 | Yes          |                     | `localparam x = [b];`                                                                                                | Verilog does not have a distinct boolean datatype. Instead, use `0` for false and `1` for true. |
|            | Bool                                  | `[b].B`                                |              | Yes                 | `[b]`                                                                                                                |                                                                                                 |
| Examples   | `val boolParam: Boolean = false`      |                                        | Yes          |                     | `localparam boolParam = 0;`                                                                                          |                                                                                                 |
|            | `true.B`                              |                                        |              | Yes                 | `1`                                                                                                                  |                                                                                                 |
| Constructs | `=`                                   | `val foo = [...]`                      | Yes          | (Indirectly)        | (aliasing operation, analogous to wire assignment in Verilog) `wire foo = [...];`                                    |                                                                                                 |
|            | `:=`                                  | `myWire := [...]`                      |              | Yes                 | `assign myWire = [...];`                                                                                             |                                                                                                 |
| Examples   | `val sum = a ^ b; val carry = a && b` |                                        | Yes          |                     | `wire sum = a ^ b; wire carry = a && b;`                                                                             |                                                                                                 |
|            | `io.out := a + b`                     |                                        |              | Yes                 | `assign out = a + b;`                                                                                                |                                                                                                 |
| Constructs | Reg                                   | `val r = RegInit([init]); r := [next]` |              | Yes                 | `reg r;`<br/> `always @ (posedge clk) begin`<br/> `  if (reset) r <= [init];`<br /> `  else r <= [next];`<br/> `end` |                                                                                                 |
|            | Wire                                  | `val w = Wire([type]); w := [value]`   |              | Yes                 | `wire [type] w; assign w = [value];`                                                                                 |                                                                                                 |

[Chisel Type Hierarchy](https://chisel.eecs.berkeley.edu/2.0.6/figs/type-hierarchy.png)
Chisel 3 supports multiple clock domains as follows.

Note that in order to cross clock domains safely, you will need appropriate synchronization logic (such as an asynchronous FIFO).

```scala
class MultiClockModule extends Module {
  val io = IO(new Bundle {
    val clockB = Input(Clock())
    val resetB = Input(Bool())
    val stuff = Input(Bool())
  })

  // This register is clocked against the module clock.
  val regClock = RegNext(io.stuff)

  withClockAndReset (io.clockB, io.resetB) {
    // In this withClock scope, all synchronous elements are clocked against io.clockB.
    // Reset for flops in this domain is using the explicitly provided reset io.resetB.

    // This register is clocked against io.clockB.
    val regClockB = RegNext(io.stuff)
  }

  // This register is also clocked against the module clock.
  val regClock2 = RegNext(io.stuff)
}
```

You can also instantiate modules in another clock domain:

```scala
class MultiClockModule extends Module {
  val io = IO(new Bundle {
    val clockB = Input(Clock())
    val resetB = Input(Bool())
    val stuff = Input(Bool())
  })
  val clockB_child = withClockAndReset(io.clockB, io.resetB) { Module(new ChildModule) }
  clockB_child.io.in := io.stuff
}
```

If you only want to connect your clock to a new clock domain and use the regular implicit reset signal, you can use `withClock(clock)` instead of `withClockAndReset`.

```scala
class MultiClockModule extends Module {
  val io = IO(new Bundle {
    val clockB = Input(Clock())
    val stuff = Input(Bool())
  })

  // This register is clocked against the module clock.
  val regClock = RegNext(io.stuff)

  withClock (io.clockB) {
    // In this withClock scope, all synchronous elements are clocked against io.clockB.

    // This register is clocked against io.clockB, but uses implict reset from the parent context.
    val regClockB = RegNext(io.stuff)
  }

  // This register is also clocked against the module clock.
  val regClock2 = RegNext(io.stuff)
}

// Instantiate module in another clock domain with implicit reset.
class MultiClockModule extends Module {
  val io = IO(new Bundle {
    val clockB = Input(Clock())
    val stuff = Input(Bool())
  })
  val clockB_child = withClock(io.clockB) { Module(new ChildModule) }
  clockB_child.io.in := io.stuff
}

```

[Prev(Polymorphism and Parameterization)](Polymorphism-and-Parameterization) [Next(Chisel3 vs Chisel2)](Chisel3-vs-Chisel2)

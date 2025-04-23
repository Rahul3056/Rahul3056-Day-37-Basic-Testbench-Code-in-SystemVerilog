

### **Day 37: Basic Testbench Code in SystemVerilog**

---

#### **1. Overview**

A **testbench** in SystemVerilog is a non-synthesizable environment used to verify the functionality of a design module (DUT – Design Under Test). It generates input stimulus, observes outputs, and checks if the DUT behaves as expected.

This is the **starting point** for any verification activity, especially in simulation. At this level, we focus on *simple, procedural testbenches*, not class-based (which comes later in UVM).

---

#### **2. Purpose of a Testbench**

- Apply **stimuli** (input signals) to the DUT  
- Observe and **record outputs**  
- Optionally **compare outputs** against expected values  
- Ensure that DUT **functions correctly under various scenarios**

---

#### **3. Basic Structure of a Testbench**

A basic testbench typically includes:

1. Declaration of testbench signals
2. Instantiation of the DUT
3. Initial block to apply input stimulus
4. Optional monitors, checkers, or display statements

---

#### **4. Example Design Module (DUT)**

We will use a simple 2-input AND gate as DUT:

```systemverilog
module and_gate (
    input  logic a,
    input  logic b,
    output logic y
);
    assign y = a & b;
endmodule
```

---

#### **5. Writing the Testbench**

```systemverilog
module tb_and_gate;

    // Declare testbench signals
    logic a, b, y;

    // Instantiate the DUT
    and_gate dut (
        .a(a),
        .b(b),
        .y(y)
    );

    // Stimulus generation
    initial begin
        $display("Starting simulation...");

        // Apply inputs and wait
        a = 0; b = 0; #10;
        $display("a=%0b, b=%0b, y=%0b", a, b, y);

        a = 0; b = 1; #10;
        $display("a=%0b, b=%0b, y=%0b", a, b, y);

        a = 1; b = 0; #10;
        $display("a=%0b, b=%0b, y=%0b", a, b, y);

        a = 1; b = 1; #10;
        $display("a=%0b, b=%0b, y=%0b", a, b, y);

        $display("Simulation complete.");
        $finish;
    end

endmodule
```

---

#### **6. Key Components Explained**

| Component       | Description                                                                 |
|----------------|-----------------------------------------------------------------------------|
| `logic`         | Data type used for signals in testbench and DUT                            |
| `initial` block | Executes once at time 0 (used for stimulus generation)                     |
| `$display`      | Prints output to the console                                               |
| `#10`           | Delay of 10 time units (used to simulate time passing)                     |
| `$finish`       | Ends the simulation                                                        |

---

#### **7. Simulation Behavior**

Simulation proceeds step-by-step:

- Inputs `a` and `b` are set
- `#10` delays allow DUT to compute output
- Output `y` is observed and printed
- Repeats for all combinations of inputs
- Simulation stops after all tests

---

#### **8. Verifying Output Automatically (Optional)**

We can add checks to automatically compare expected values:

```systemverilog
initial begin
    a = 0; b = 0; #10; if (y !== 0) $error("Mismatch: 0 & 0 should be 0");
    a = 0; b = 1; #10; if (y !== 0) $error("Mismatch: 0 & 1 should be 0");
    a = 1; b = 0; #10; if (y !== 0) $error("Mismatch: 1 & 0 should be 0");
    a = 1; b = 1; #10; if (y !== 1) $error("Mismatch: 1 & 1 should be 1");

    $display("All test cases passed.");
    $finish;
end
```

---

#### **9. Best Practices**

- **Isolate testbench** from DUT logic.
- Use meaningful **signal and module names**.
- Include both **stimulus and output checking**.
- Keep stimulus **readable and sequenced**.
- Don’t mix **blocking (`=`)** and **non-blocking (`<=`)** inappropriately.
- Use **`$display` and `$monitor`** to debug effectively.
- For larger designs, move toward **modular TB structure** (drivers, monitors, checkers).

---

#### **10. Summary**

- A testbench drives and observes the DUT to verify correctness.
- Use `initial` blocks for stimulus and `$display`/`$monitor` to observe results.
- Always instantiate the DUT inside the testbench and connect testbench signals.
- Start with simple TBs and build toward more structured verification.

---

Let me know when you're ready to continue with **Day 38: Assertions – Basics**.

# Turing-Complete
A NAND-to-CPU systems-building project that constructs a full processor—from logic gates to a custom assembler—to explore computing from the ground up.

ADDING BYTES:
Two input values are split into 8-bit buses and combined using a hierarchy of adders. Partial sums are grouped from lower to higher significance, with carries propagated accordingly. A final overflow carry from the most significant bit is handled by a dedicated component.

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/ddc122bf-6385-4b50-bd57-74142a297079" />

SAVING BYTES:
This step introduces a simple byte storage mechanism.
The input value is split into an 8-bit bus, where each bit is routed to a dedicated storage component. Each component has two inputs: one for the bit signal itself, and one save control signal. A bit is updated only when both the bit signal and the save signal are active; otherwise, the previously stored value is preserved.
Each stored bit is then forwarded to its corresponding position on an output byte bus. The output of this bus is exposed only when a separate load control signal is activated, allowing the stored byte to be read explicitly.

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/acde1578-e51f-425d-a482-859f0e265aa9" />

ARITHMETIC UNIT LOGIC - VERSION 1:
This first version of the arithmetic and logic unit (ALU) implements a basic logical operation pipeline based on an operation code.

The unit takes two 8-bit input values, which are distributed across internal buses and routed through a network of logic gates. An operation code selects the active computation path by enabling specific sections of the circuit.

In this version, the ALU focuses on implementing a NAND operation. Each bit of the two input values is processed independently through a set of logic gates, producing an 8-bit result. The operation is fully combinational: no internal state is stored, and the output reflects the current inputs and selected operation.

The final result is assembled onto an output byte bus, allowing the computed value to be compared against the expected result. This structure establishes the foundation for extending the ALU with additional arithmetic and logical operations in future iterations.
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/5c0d2245-03a5-40d7-8aaa-dee9b70bd534" />

ARITHMETIC UNIT LOGIC - VERSION 2: 
This step implements a basic arithmetic logic module capable of performing addition and subtraction.

The unit accepts two multi-bit inputs and a code signal specifying the operation (e.g., ADD or SUB). The inputs are routed through a combination of XOR, AND, OR, NAND, and NOR gates to handle bitwise operations and carry propagation. Conditional logic blocks manage operation selection, ensuring the correct arithmetic function is applied. Intermediate results are then combined and passed through sequential storage elements to produce the final output. The output bus displays the computed result only after the operation is complete, demonstrating controlled arithmetic computation at the bitwise level and forming the core of low-level processing in a CPU.
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/e31957ea-2bfb-48d9-925f-af73532ca2aa" />

REGISTER:
This register module provides temporary storage for 8-bit values within the system. It receives an input byte from the data bus and stores it when the write control signal is enabled. It register is built from independent bit-storage elements, allowing each bit of the input value to be latched simultaneously on a control event. Once stored, the value remains stable and can be placed back onto the output bus when the read signal is activated.

The design is synchronous and stateful: unlike the ALU, the register preserves its value across cycles until it is explicitly updated. This behavior makes it suitable for holding intermediate results, operands, or addresses during program execution.
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/5871cd35-1fe9-4f8c-a66d-bfd92e6842c5" />

INSTRUCTION DECODER:
This instruction decoder translates the encoded instruction byte into explicit control signals used by the datapath.

The decoder receives the instruction opcode and splits it into individual bit lines. A set of logic gates is then used to detect specific instruction patterns by combining selected bits through AND and NOT operations. Each detection path corresponds to a single instruction type. When a valid opcode is recognized, the associated output control line is asserted, enabling the appropriate subsystem (such as computation, data movement, or conditional logic). Only one instruction line is active at a time, ensuring unambiguous control flow.
<img width="1920" height="1080" alt="Turing Complete Decodeur d&#39;instruction" src="https://github.com/user-attachments/assets/dfd2ee76-37e1-4cdc-8a43-58f33b844d64" />

CONDITIONS:
This condition evaluation module determines whether a logical condition is satisfied based on the current input value.

The circuit analyzes an 8-bit signed value by combining bit decoding, sign detection, and zero detection logic. Dedicated logic paths identify specific conditions such as equality to zero or sign-based comparisons. The input value is first decomposed into individual bit signals, which are then recombined using AND, OR, and NOT gates to form higher-level condition flags.

The output signal is asserted only when the selected condition evaluates to true. This module is fully combinational and does not store state, ensuring that condition results always reflect the current input value
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/5f86ba4a-696e-4675-b685-c7e453ef8810" />

Immediate values:
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/777320ae-236b-4f2b-b843-be2e144ea52f" />

Turing complete:
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/d20a63c5-1205-4816-baa3-6724ed023b86" />








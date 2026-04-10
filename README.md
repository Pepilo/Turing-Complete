# Turing-Complete
A NAND-to-CPU systems-building project that constructs a full processor—from logic gates to a custom assembler—to explore computing from the ground up.

## Adding bytes

Two input values are split into 8-bit buses and combined using a hierarchy of adders. Partial sums are grouped from lower to higher significance, with carries propagated accordingly. A final overflow carry from the most significant bit is handled by a dedicated component.

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/ddc122bf-6385-4b50-bd57-74142a297079" />

## Saving bytes

This step introduces a simple byte storage mechanism.
The input value is split into an 8-bit bus, where each bit is routed to a dedicated storage component. Each component has two inputs: one for the bit signal itself, and one save control signal. A bit is updated only when both the bit signal and the save signal are active; otherwise, the previously stored value is preserved.
Each stored bit is then forwarded to its corresponding position on an output byte bus. The output of this bus is exposed only when a separate load control signal is activated, allowing the stored byte to be read explicitly.

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/acde1578-e51f-425d-a482-859f0e265aa9" />

## Arithmetic Unit Logic - v1

This first version of the arithmetic and logic unit (ALU) implements a basic logical operation pipeline based on an operation code.

The unit takes two 8-bit input values, which are distributed across internal buses and routed through a network of logic gates. An operation code selects the active computation path by enabling specific sections of the circuit.

In this version, the ALU focuses on implementing a NAND operation. Each bit of the two input values is processed independently through a set of logic gates, producing an 8-bit result. The operation is fully combinational: no internal state is stored, and the output reflects the current inputs and selected operation.

The final result is assembled onto an output byte bus, allowing the computed value to be compared against the expected result. This structure establishes the foundation for extending the ALU with additional arithmetic and logical operations in future iterations.

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/5c0d2245-03a5-40d7-8aaa-dee9b70bd534" />

## Arithmetic Unit Logic - v2

This step implements a basic arithmetic logic module capable of performing addition and subtraction.

The unit accepts two multi-bit inputs and a code signal specifying the operation (e.g., ADD or SUB). The inputs are routed through a combination of XOR, AND, OR, NAND, and NOR gates to handle bitwise operations and carry propagation. Conditional logic blocks manage operation selection, ensuring the correct arithmetic function is applied. Intermediate results are then combined and passed through sequential storage elements to produce the final output. The output bus displays the computed result only after the operation is complete, demonstrating controlled arithmetic computation at the bitwise level and forming the core of low-level processing in a CPU.

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/e31957ea-2bfb-48d9-925f-af73532ca2aa" />

## Register
This register module provides temporary storage for 8-bit values within the system. It receives an input byte from the data bus and stores it when the write control signal is enabled. It register is built from independent bit-storage elements, allowing each bit of the input value to be latched simultaneously on a control event. Once stored, the value remains stable and can be placed back onto the output bus when the read signal is activated.

The design is synchronous and stateful: unlike the ALU, the register preserves its value across cycles until it is explicitly updated. This behavior makes it suitable for holding intermediate results, operands, or addresses during program execution.

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/5871cd35-1fe9-4f8c-a66d-bfd92e6842c5" />

## Instruction decoder

This instruction decoder translates the encoded instruction byte into explicit control signals used by the datapath.

The decoder receives the instruction opcode and splits it into individual bit lines. A set of logic gates is then used to detect specific instruction patterns by combining selected bits through AND and NOT operations. Each detection path corresponds to a single instruction type. When a valid opcode is recognized, the associated output control line is asserted, enabling the appropriate subsystem (such as computation, data movement, or conditional logic). Only one instruction line is active at a time, ensuring unambiguous control flow.

<img width="1920" height="1080" alt="Turing Complete Decodeur d&#39;instruction" src="https://github.com/user-attachments/assets/dfd2ee76-37e1-4cdc-8a43-58f33b844d64" />

## Conditions

This condition evaluation module determines whether a logical condition is satisfied based on the current input value.

The circuit analyzes an 8-bit signed value by combining bit decoding, sign detection, and zero detection logic. Dedicated logic paths identify specific conditions such as equality to zero or sign-based comparisons. The input value is first decomposed into individual bit signals, which are then recombined using AND, OR, and NOT gates to form higher-level condition flags.

The output signal is asserted only when the selected condition evaluates to true. This module is fully combinational and does not store state, ensuring that condition results always reflect the current input value.

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/5f86ba4a-696e-4675-b685-c7e453ef8810" />

## Immediate value

This module implements immediate constant loading within the architecture.

The circuit allows fixed 8-bit values encoded in the program memory to be routed directly into the data path. Instruction bits are decoded to select and propagate immediate values through a dedicated multiplexing network. These constants can then be injected into the internal bus and used by registers or the ALU without requiring prior memory access.

The design relies on combinational decoding logic and bus gating to ensure that only the selected immediate value drives the bus at a given cycle. This component extends the instruction set by enabling literal operands, which is essential for arithmetic operations, comparisons, and control flow initialization.

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/777320ae-236b-4f2b-b843-be2e144ea52f" />

## Conditional execution

This step integrates condition evaluation into calculation opcodes.

The architecture now combines arithmetic operation decoding with condition flags produced by the condition module. Each calculation instruction can be selectively enabled or suppressed based on the current condition result. Logical gating ensures that ALU operations and register writes only occur when the specified condition evaluates to true.

By merging opcode decoding with conditional control signals, the processor supports conditional arithmetic without dedicated branch instructions. This mechanism is a key building block toward full control flow expressiveness and contributes directly to achieving Turing completeness.

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/d20a63c5-1205-4816-baa3-6724ed023b86" />

# ⭐ Turing Completeness

At this stage, the architecture reaches full Turing completeness.

The system now combines arithmetic operations, immediate value injection, conditional evaluation, and conditional execution within a unified control path. Data can be modified through the ALU, stored in registers, and reused across cycles, providing persistent state. Conditional logic allows operations to be enabled or suppressed based on computed values, enabling decision-making within programs.

By supporting conditional arithmetic and stateful computation, the architecture can implement loops, emulate conditional branches, and construct arbitrary control flow through instruction sequencing. These capabilities are sufficient to simulate a Turing machine, making the processor computationally universal.

This milestone marks the transition from a fixed-function datapath to a fully programmable system capable of expressing any computable algorithm within its resource limits.

## Binary editor

In this stage, the program is written directly in binary form using the built-in memory editor. Each instruction is encoded as a sequence of bytes where the bits represent:

- the operation code (opcode) that defines which action the CPU must perform,

- the memory addresses involved in the operation,

- the values or registers used by the instruction.

The goal of the level is simple: add 5 to the input value.

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/e6e95abb-66cd-4ed1-9582-205660104410" />

## Assembly language

In this stage, the program is no longer written directly in raw binary. Instead, a custom assembler layer is introduced to make programs easier to read and write.

However, unlike traditional assembly languages, the instructions are not predefined. I must manually define them by associating a sequence of bits with a human-readable instruction name.

This means that each instruction in the assembler corresponds to a specific binary pattern that will be emitted into memory when the program is assembled.

The workflow is therefore:

Define instructions
Each instruction (such as add, AND, or register transfers) is created by assigning it a binary code. This binary sequence represents the low-level control signals executed by the CPU.

Create symbolic names
These binary patterns are given meaningful names so they can be reused in the program. This effectively builds a small custom instruction set.

Write the program using the defined instructions
Once defined, the instructions can be used to build logic using labels, constants, and control flow.

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/255a563f-aae2-4021-a5f5-de64e12c920a" />

## Maze solving in assembly

In this stage, the goal is to program a robot to autonomously navigate and escape a maze using the custom assembler built in the previous step.

The challenge is no longer about defining instructions, but about using them effectively to implement a navigation strategy under low-level constraints. The robot interacts with its environment through limited inputs (such as wall detection) and must decide its movements accordingly.

To solve this, I implemented a control loop that continuously:

Reads the environment (e.g., detecting walls in specific directions)
Stores intermediate state in registers
Chooses the next movement (turn left, right, or move forward)
Executes the corresponding action

The program relies heavily on conditional jumps and register-based logic to adapt to the maze structure. Labels are used to organize the flow (looping, turning logic, decision branches), effectively recreating higher-level control structures in assembly.

This stage highlights how even simple behaviors—like following a wall or choosing a direction—require careful handling when working at such a low level. It also demonstrates how the custom instruction set can be composed to build more complex, stateful behavior.

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/5c09353b-305e-4f1c-9c93-6311f8fe28ff" />

## Raw binary programming

In this stage, the abstraction layer of the assembler is removed, and the program is written directly using raw binary instructions again.

Unlike previous steps where instructions had symbolic names, I now have to manipulate the exact byte values stored in memory. Each number directly represents an instruction or data, making the program much less readable but closer to how the machine actually operates.

The objective here is to implement a timed behavior based on clock cycles:

During even cycles, the system reads and stores incoming data into memory
During odd cycles, it outputs two bytes at a time

To achieve this, I rely on low-level components such as logic gates and counters to detect cycle parity (even vs odd), and control when data should be written or sent to the output.

This stage emphasizes the transition from a human-friendly representation (assembler) back to pure machine-level control. It highlights how timing, data flow, and control signals must be explicitly managed without the help of higher-level abstractions.

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/68d6f37c-940c-40a0-9e3e-12a85f2ef603" />

## LED architecture design

In this stage, the focus shifts from programming to hardware architecture design, following a LED-based (Little Endian Data) structure to organize data flow and computation.

Instead of writing instructions, I now build the underlying datapath of the system:

Registers are implemented to store intermediate values
Buses are used to route data between components
Control signals orchestrate when data is read, written, or transformed

A key part of this step is implementing the ADD opcode directly at the hardware level. This involves wiring together logic components to perform binary addition and ensuring the result is correctly propagated through the system.

The resulting circuit becomes significantly more complex, with many interconnected components (hence the “spaghetti” structure). Each connection represents a precise data or control dependency that must be carefully managed.

This stage highlights how higher-level instructions ultimately rely on well-structured hardware foundations, and how even a simple operation like addition requires coordinated interactions between multiple low-level components.

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/eaf6b0b1-6ec1-4eba-8d64-39e2e1351262" />

## Conditional logic in LED architecture

In this stage, conditional execution is introduced into the LED-based architecture, allowing the system to make decisions based on computed values.

Until now, the datapath could only execute instructions sequentially. With the addition of conditional logic, the architecture gains the ability to alter control flow depending on specific conditions (such as comparisons or register values).

To achieve this, I implemented:

Comparison logic using basic gates (AND, OR, NOT) to evaluate conditions
Signal routing that determines whether a branch or instruction should be executed
Control lines that enable or disable parts of the circuit based on these conditions

This effectively adds branching capabilities at the hardware level, similar to if statements in higher-level languages. The circuit now decides dynamically which path to follow, rather than executing everything linearly.

As a result, the design becomes even more intricate, with additional layers of control logic intertwined with the datapath. This stage highlights how fundamental concepts like conditionals emerge from relatively simple logic components when combined at scale.

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/33f67147-c880-4461-ad4c-6f6f666bb847" />

## Bitwise shift in LED architecture

In this stage, I implement a bitwise shift operation directly at the hardware level within the LED-based architecture.

The goal is to shift an input value to the left by a variable number of positions specified by a second input. Unlike fixed shifts, this requires dynamic routing of bits depending on the shift amount.

To achieve this, I designed:

A network of multiplexers to select the correct shifted output
Wiring patterns that map each input bit to multiple possible output positions
Control logic that activates the appropriate paths based on the shift value

This effectively creates a barrel shifter, where the input can be shifted by several positions in a single operation rather than through iterative steps.

The circuit becomes highly interconnected, as each bit must be able to propagate to multiple destinations. This results in a dense wiring structure, but allows for efficient, parallel bit manipulation.

This stage demonstrates how more advanced operations, often taken for granted in high-level programming, require carefully structured and scalable hardware designs to be implemented efficiently.

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/42149af4-654e-4bb9-95bd-5d2da55beb56" />

## Integration of RAM into the LED architecture

In this stage, I integrate a RAM module into the LED-based architecture, enabling the system to store and retrieve data dynamically during execution.

Until now, the architecture could only rely on registers and fixed program memory. With the addition of RAM, it gains the ability to handle larger and mutable datasets, which is essential for more complex programs.

To implement this, I designed:

Addressing logic to select specific memory locations
Read/write control signals to determine when data is stored or retrieved
Data buses connecting the RAM to the rest of the datapath

The RAM must be carefully synchronized with the rest of the system to ensure correct timing between memory access and instruction execution.

This significantly increases the complexity of the circuit, as memory operations introduce new dependencies between control flow and data flow.

This stage marks an important milestone, as the architecture now resembles a more complete computing system, capable of executing programs that rely on persistent and addressable memory rather than only immediate values.

<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/8597294b-993b-4798-9d06-428f8a408e5b" />

## Stack Implementation using RAM

<img width="1909" height="1076" alt="image" src="https://github.com/user-attachments/assets/4791ebba-e9ea-4326-a127-554247a2042f" />















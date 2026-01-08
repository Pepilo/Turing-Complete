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

Space-constrained memory :
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/bb39aeba-4bac-4f4e-bbb0-5fcb8ec2f62b" />

Arithmetic logic unit - Version 2:
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/e31957ea-2bfb-48d9-925f-af73532ca2aa" />

Register:
<img width="1920" height="1080" alt="image" src="https://github.com/user-attachments/assets/5871cd35-1fe9-4f8c-a66d-bfd92e6842c5" />

Instruction decoder:
<img width="1920" height="1080" alt="Turing Complete Decodeur d&#39;instruction" src="https://github.com/user-attachments/assets/dfd2ee76-37e1-4cdc-8a43-58f33b844d64" />





### Okayyy Let's GoO (Reverse Engineering Day)
```bash
.Stack is a section of memory reserved for temporaru data storage, managed by the system to support program execution.
.Last in, first out (LIFO)

### x86_64 Assembly common terms
Heap: Memory that can be allocated and deallocated
Stack: Contigeous section of memory used for passing arguments
General Register: Multipurpose register than can be used by either a programmer or user to store data or a memory location address
Control Register: A processor register that changes or controls the behavior of a cpu
Flags Register: Contains the current state of the processor
## Register:  a register is a small, extremely fast memory location within the processor (CPU) that temporarily stores data, instructions, and memory addresses needed for immediate processing. Registers act as a critical, high-speed storage buffer, allowing the CPU to access and manipulate vital information in a single clock cycle, which greatly speeds up overall system performance and enables complex calculations
.On an x86_64 Assembly register, There are 16 general purpose 64-bit registers.
### UGHHH
MOV: move to destination from source
mov r15,#n  <- r15 = #n where #n is a specified value to be moved into r15
mov r15,r13 <- the value of r13 is copied into r15. The value in r13 is not changed.
mov rax,m   <- move contents of 64bit address to %rax
mov m,rax   <- move contents of %rax to 64 bit memory address

PUSH: push source onto stack. This is a special form of MOV where the destination is always the top of the stack.
push r15    <-push r15 onto stack. The stack pointer %rsp then updates to the location of the new top of the stack.

POP: Pop top of stack to destination. This is a special form of MOV where the source is always the top of the stack.
pop r8      <-move value on top of stack to r8 and update the stack pointer. 

INC: Increment source by 1
inc r8      <-increment value in r8 by 1

DEC: Decrement source by 1
dec r8      <-decrement value in r8 by 1

ADD: Add source to destination
add r13,#n  <-add #n to %r13, store result in %r13
add r13,r12  <-add the value in %r12 to %r13, store result in %r13. %r12 is not changed by this action.

SUB: Subtract source from destination
sub r13,#n  <-subtract #n from %r13, store result in %r13
sub r13,r12  <-subtract the value in %r12 from %r13, store result in %r13. %r12 is not changed by this action.

CMP: Compare 2 values by subtracting them and setting the %RFLAGS register. ZeroFlag set means they are the same. SignFlag set means the result of the comparison is the first register being less than the second register.
cmp r8, r9  <- compare value of r8 to r9. Set flags as appropriate.

JMP: Jump to specified location
jmp MEM1    <-jump to memory label MEM1

JLE: Jump if less than or equal
jle MEM1    <-jump to memory label MEM1 if less than or equal

JE: Jump if equal
je MEM1     <-jump to memory label MEM1 if equal

Understand the x86_64 stack

### DEMOS




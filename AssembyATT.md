# AT&T x86_64 Assembly Language Reference Guide

## Table of Contents
1. [AT&T vs Intel Syntax](#syntax)
2. [Registers](#registers)
3. [Memory Addressing](#memory)
4. [Instructions](#instructions)
5. [Jumps and Control Flow](#jumps)
6. [Examples](#examples)
7. [GDB](#gdb)

## Syntax Differences <a name="syntax"></a>

### AT&T vs Intel Key Differences
- Prefixes: AT&T uses `%` for registers, `$` for immediate values
- Direction: AT&T is source→destination, Intel is destination←source
- Memory references: AT&T uses `()` for memory references
- Size suffixes: AT&T adds b/w/l/q to instructions (e.g., `movq` vs Intel `mov`)

Example:
```gas
# AT&T Syntax
movq $42, %rax    # Move immediate 42 into rax
movq (%rsp), %rdi # Move value at address in rsp to rdi

; Intel Equivalent
mov rax, 42       ; Move immediate 42 into rax
mov rdi, [rsp]    ; Move value at address in rsp to rdi
```

## Registers <a name="registers"></a>

### General Purpose Registers

| 64-bit | 32-bit | 16-bit | 8-bit | Purpose | System V AMD64 ABI |
|--------|--------|---------|-------|---------|-------------------|
| %rax   | %eax   | %ax     | %al   | Accumulator/Return | Return value |
| %rbx   | %ebx   | %bx     | %bl   | Base | Callee-saved |
| %rcx   | %ecx   | %cx     | %cl   | Counter | 4th argument |
| %rdx   | %edx   | %dx     | %dl   | Data | 3rd argument |
| %rsi   | %esi   | %si     | %sil  | Source Index | 2nd argument |
| %rdi   | %edi   | %di     | %dil  | Destination Index | 1st argument |
| %rbp   | %ebp   | %bp     | %bpl  | Base Pointer | Callee-saved |
| %rsp   | %esp   | %sp     | %spl  | Stack Pointer | Stack Pointer |
| %r8    | %r8d   | %r8w    | %r8b  | General | 5th argument |
| %r9    | %r9d   | %r9w    | %r9b  | General | 6th argument |
| %r10   | %r10d  | %r10w   | %r10b | General | Caller-saved |
| %r11   | %r11d  | %r11w   | %r11b | General | Caller-saved |
| %r12   | %r12d  | %r12w   | %r12b | General | Callee-saved |
| %r13   | %r13d  | %r13w   | %r13b | General | Callee-saved |
| %r14   | %r14d  | %r14w   | %r14b | General | Callee-saved |
| %r15   | %r15d  | %r15w   | %r15b | General | Callee-saved |

### Special Purpose Registers
- %rip: Instruction Pointer
- %rflags: Flags Register (status flags)
- %cs, %ds, %ss, %es, %fs, %gs: Segment registers

## Memory Addressing <a name="memory"></a>

### Basic Addressing Modes
```gas
# Direct
movq value, %rax     # Load from memory location 'value'

# Register Indirect
movq (%rax), %rbx    # Load from address in %rax

# Register Relative
movq 8(%rax), %rcx   # Load from address %rax + 8

# Scaled Index
movq (%rax,%rbx,4), %rdx  # Load from %rax + %rbx * 4

# Full SIB (Scale-Index-Base)
movq 8(%rax,%rbx,4), %rdi # Load from 8 + %rax + %rbx * 4
```

### Stack Operations
```gas
pushq %rax           # Push %rax onto stack
popq %rbx           # Pop top of stack into %rbx
```

## Instructions <a name="instructions"></a>

### Data Movement
| Instruction | Description | Example |
|-------------|-------------|---------|
| mov         | Move data   | `movq %rax, %rbx` |
| lea         | Load effective address | `leaq (%rax,%rbx,4), %rcx` |
| xchg        | Exchange    | `xchgq %rax, %rbx` |

### Arithmetic
| Instruction | Description | Example |
|-------------|-------------|---------|
| add         | Add         | `addq $1, %rax` |
| sub         | Subtract    | `subq $4, %rsp` |
| mul         | Unsigned multiply | `mulq %rbx` |
| div         | Unsigned divide   | `divq %rcx` |
| inc         | Increment   | `incq %rax` |
| dec         | Decrement   | `decq %rbx` |

### Logical
| Instruction | Description | Example |
|-------------|-------------|---------|
| and         | Bitwise AND | `andq $0xF, %rax` |
| or          | Bitwise OR  | `orq %rbx, %rax` |
| xor         | Bitwise XOR | `xorq %rax, %rax` |
| not         | Bitwise NOT | `notq %rax` |
| shl/sal     | Shift left  | `shlq $1, %rax` |
| shr         | Shift right | `shrq $2, %rbx` |

## Jumps and Control Flow <a name="jumps"></a>

### Conditional Jumps
| Instruction | Description | Condition |
|-------------|-------------|-----------|
| je/jz       | Jump if equal/zero | ZF=1 |
| jne/jnz     | Jump if not equal/not zero | ZF=0 |
| jg/jnle     | Jump if greater (signed) | ZF=0 & SF=OF |
| jge/jnl     | Jump if greater/equal (signed) | SF=OF |
| jl/jnge     | Jump if less (signed) | SF≠OF |
| jle/jng     | Jump if less/equal (signed) | ZF=1 or SF≠OF |
| ja/jnbe     | Jump if above (unsigned) | CF=0 & ZF=0 |
| jae/jnb     | Jump if above/equal (unsigned) | CF=0 |
| jb/jnae     | Jump if below (unsigned) | CF=1 |
| jbe/jna     | Jump if below/equal (unsigned) | CF=1 or ZF=1 |

### Special Jumps
```gas
jmp label           # Unconditional jump
jmp *%rax           # Indirect jump through register
jmp *(%rax)         # Indirect jump through memory
notrack jmp label   # Jump without BTB prediction
```

## Examples <a name="examples"></a>

### Basic Function
```gas
    .global main
main:
    pushq %rbp          # Save old base pointer
    movq %rsp, %rbp     # Set up new base pointer
    
    # Function body
    movq $42, %rax      # Return value
    
    movq %rbp, %rsp     # Restore stack pointer
    popq %rbp           # Restore base pointer
    ret                 # Return
```

### Array Access
```gas
    # Accessing array[i] where array is at offset -16(%rbp)
    # and i is in %rcx
    leaq -16(%rbp), %rax    # Get array base address
    movq (%rax,%rcx,8), %rdx # Load array[i] (assuming 8-byte elements)
```

### Conditional Execution
```gas
    cmpq $0, %rax       # Compare rax with 0
    jle .L_negative     # Jump if less than or equal
    
    # Code for positive case
    movq $1, %rbx
    jmp .L_end
    
.L_negative:
    # Code for negative case
    movq $-1, %rbx
    
.L_end:
    # Continue execution
```

### Stack Frame Management
```gas
    # Prologue
    pushq %rbp
    movq %rsp, %rbp
    subq $16, %rsp      # Allocate 16 bytes of stack space
    
    # Access local variables
    movq $42, -8(%rbp)  # Store 42 in first local variable
    movq -8(%rbp), %rax # Load first local variable
    
    # Epilogue
    movq %rbp, %rsp
    popq %rbp
    ret
```

### Register vs Memory Access
```gas
    # Direct register access
    movq %rax, %rbx     # Move value in rax to rbx
    
    # Memory access through register
    movq (%rsp), %rax   # Load value at address in rsp
    
    # Memory access with offset
    movq 8(%rsp), %rax  # Load value at rsp+8
    
    # Memory access with base and index
    movq (%rbx,%rcx,8), %rax # Load from rbx + rcx*8
```


## GDB Debugging Guide <a name="gdb"></a>

### Basic GDB Commands
```
# Starting GDB
gdb ./program              # Start GDB with program
gdb ./program core         # Debug core dump
gdb ./program --pid 1234   # Attach to running process

# Execution Control
run (r)                    # Start program execution
start                      # Run to main()
continue (c)              # Continue execution
next (n)                  # Step over
nexti (ni)                # Step over (instruction level)
step (s)                  # Step into
stepi (si)                # Step into (instruction level)
finish                    # Run until current function returns
break (b) main           # Set breakpoint at main
break *0x400500          # Set breakpoint at address
watch *0x400500          # Watch memory location
delete 1                  # Delete breakpoint #1
```

### Memory Examination

#### Format Specifiers
```
x/FMT ADDRESS   # Examine memory
                # FMT = [COUNT][SIZE][FORMAT]

# Count: Number of units to display
x/3x $rsp       # Show 3 units

# Size specifiers:
b - byte (1)    # x/b
h - half (2)    # x/h
w - word (4)    # x/w
g - giant (8)   # x/g

# Format specifiers:
x - hex         # x/x
d - decimal     # x/d
u - unsigned    # x/u
o - octal       # x/o
t - binary      # x/t
f - float       # x/f
a - address     # x/a
i - instruction # x/i
c - char        # x/c
s - string      # x/s

# Common combinations:
x/32xb $rsp     # 32 bytes in hex from stack pointer
x/8gx $rsp      # 8 giant (8-byte) words in hex
x/s $rdi        # String pointed to by rdi
x/10i $rip      # Next 10 instructions from current IP
```

### Register Access
```
# Print registers
info registers (i r)     # All registers
info registers rax rbx   # Specific registers

# Print specific register
p $rax                  # Print rax in decimal
p/x $rax               # Print rax in hex
p/t $rax               # Print rax in binary

# Access register-relative addresses
x/gx $rsp              # Value at rsp
x/gx $rsp+8            # Value 8 bytes above rsp
x/gx $rsp-8            # Value 8 bytes below rsp
x/4gx $rsp             # 4 values starting at rsp
```

### Memory Navigation
```
# Examining adjacent memory
x/8gx $rsp-32          # 8 quadwords before rsp
x/16i $rip             # Next 16 instructions
x/8gx &variable        # 8 quadwords starting at variable

# Finding specific values
find /w 0x400000, 0x401000, 0x12345678  # Find word in range
find /s 0x400000, 0x401000, "string"    # Find string in range

# Memory mapping
info proc mappings     # Show memory map
maintenance info sections # Show section addresses
```

### Stack Frame Analysis
```
# Frame information
bt (backtrace)         # Show call stack
frame (f) 2            # Select frame #2
info frame             # Current frame details
info locals            # Local variables
info args              # Function arguments

# Stack examination
x/32gx $rsp           # Examine 32 stack qwords
x/gx $rbp-8           # Local variable at rbp-8
x/gx $rbp+16          # Argument above frame
```

### Data Structure Navigation
```
# Following pointers
p *((struct name *)$rax)  # Cast and dereference
p/x *(int *)($rsp+8)     # Type cast memory
x/gx *($rax+8)           # Follow pointer with offset

# Array access
x/10gx array             # Show 10 elements
p array[5]               # Print 6th element
p/x *array@10           # Print 10 elements
```

### Advanced Features
```
# Conditional breakpoints
break main if argc == 2
watch *0x400500 if $rax == 0

# Python scripting
python
def print_rax():
    rax = gdb.selected_frame().read_register("rax")
    print(f"RAX = {rax}")
end

# Custom commands
define print_stack
    x/16gx $rsp
end

# Logging
set logging on
set logging file output.txt
```

### Common Debugging Patterns
```
# Following function calls
start
b *function
commands
    print $rdi    # Print first argument
    continue
end

# Memory leak checking
watch malloc
commands
    backtrace
    continue
end

# Tracking register changes
watch $rax
commands
    backtrace
    p/x $rax
    continue
end
```

### Tips and Tricks
```
# Layout
layout asm               # Assembly view
layout regs             # Registers view
ctrl-x a                # TUI mode toggle

# Convenience variables
set $i = 0              # Set counter
print ++$i              # Use in commands

# Aliases
alias pra = p/x $rax
alias prb = p/x $rbx

# Command history
show commands           # Show recent commands
!123                    # Repeat command #123
```

### Common Debugging Workflows

#### Finding Memory Corruption
```
# Watch memory range
watch *(int *)0x400500
commands
    bt
    p/x $rax
    x/32gx $rsp
    continue
end

# Track writes to memory
catch syscall write
commands
    bt
    x/s $rsi
    continue
end
```

#### Analyzing Crashes
```
# Start with core file
gdb ./program core
where                   # Show crash location
frame 0                 # Go to crash frame
x/16i $rip-32          # See instructions before crash
info registers         # Register state at crash
x/32gx $rsp           # Examine stack
```


+++
title = "Introduction to Assembly for macOS ARM64"
description = "Basic introduction to Assembly programming for macOS ARM64 (M1)"
+++

It is incredible how we can write programs on any programming language, and the CPU is able to process those
instructions internally. Even if you don't work with low level programming, it is still fascinating to understand how
the CPU executes instructions and how data is managed on memory.

Learning [C][c_lang] already covers many important concepts, such as direct memory management. However, it still
abstracts some lower level details handled internally by the C compiler. That is where Assembly comes in, it is the next
step toward lower level language where developers have no standard libraries or utility functions. Additionally,
Assembly is so low level it differs for each CPU architecture the program is targeted to run on.

Assembly can be very intimidating for beginners and may even seem completely unreasonable or impossible to understand at
first. I certainly felt that way for a long time. While it is not as easy to learn as higher level languages, like any
other language, with time, dedication, and practice, things start to make sense.

To be honest, learning Assembly for ARM64 on MacOS wasn't as straightforward as it might be on other operating systems.
Apple doesn't provide much documentation on assembly, and many existing resources are outdated. However, searching well
across the internet and piecing together information from different sources, it is possible to find everything we need.

As always, when learning a new language, we start with a "Hello, World!" example. Lets first take a look on the code and
then break down how it works:

```asm
; Based on example from https://www.youtube.com/watch?v=9-rgo57Ew2g
.global _main

_main:
    mov X0, #1          ; arg[0] = 1 (STDOUT)
    adr X1, helloworld  ; arg[1] = string to print
    mov X2, #15         ; arg[2] = length of our string
    mov X16, #4         ; Unix write system call
    svc 0               ; call kernel for the above syscall

    mov X0, #0          ; Use 0 return code
    mov X16, #1         ; Unix exit system call
    svc 0               ; call kernel for the above syscall

helloworld:
    .ascii "Hello, World!\n"
```

Running it:

```shell
# Compile our assembly source code to a object file
as hello.s -o hello.o

# Build a binary for our object file
ld hello.o -o hello -l System -syslibroot `xcrun -sdk macosx --show-sdk-path` -e _main -arch arm64
./hello
```

Yay! If everything worked as expected, you should see a "Hello, World!" messaged printed in your terminal.

Now, lets break down how it works. However, instead of going through it line by line, we will take a different approach by grouping the
explanation based on identifier types:

### Directives

Identifiers starting with point (`.`) represent directives. They are not translated into machine code and they are used
by the assembler on assembling a program to take some action or change a setting.

- `.global <name>`: Makes a label visible outside of the program for the linker. Therefore, using `.global _main` in our
  program, and `-e _main` in later in the linker (`ld` command), will make `_main` the entry point for our program.
  Without the `.global` directive that wouldn't be accessible externally.
- `.ascii <string>`: It assembles each string character into a consecutive address, as we would expect for a string data
  stored in memory.

## Labels

Identifiers ending with colon (`:`) represent labels. They are basically name for lines, and can be referenced by other
parts of the program, such as to jump to a different location or to load data from a location.

- `_main:`: is used in this example to mark where our program begins.
- `helloworld:`: it points to the `ascii` data for the hello world message.

## Immediate values

Identifiers starting with `#` represent immediate values (unless it is used out of the program area, then it could be a
comment). They are values [hard coded][hard_coding] into the assembly source code program, instead of loading the data
from other sources such as from stack or heap address. Think on them just like is done in any other programming language
when we set a variable with a hard code value.

- `#1`: means we are passing number one as argument to something. Keep in mind in assembly numbers must be on
  hexadecimal format, so beyond 9, numbers start to include A-F letters. For instance, `8=8`, `9=9`, `10=A`, `11=B`, `15=F`,
  `16=10`, and so on.

## Registers

Registers are quickly accessible locations for the CPU. They are normally at the top of the [memory hierarchy][memory_hierarchy], and
provide the fastest way for the processor to access data. Whenever something needs to be processed by the CPU, we use
those registers as input/output parameters for CPU operations.

On ARM64 there are 31 64-bit integer registers labeled `X0` to `X30` and the CPU has specific usage for each one of
them. In our example we use general purpose registers `X0`, `X1`, `X2`, and `X16`. The reason we are using those
specific registers is based on the system call we want the kernel to run for us, those details are covered in the next
sections.

It is worth mentioning that `X16` is a reserved register used by ARM64 processor, it tells which system call we are about to
execute. It is usually specified right before we a `svc` call. More details about this on the System Calls section.

## Instructions

Instructions are used to control the CPU. They are basically commands the CPU will execute such as arithmetic operations
(add, subtract, multiply, and etc.), move memory around, change the kernel mode privilege to run syscalls, and many
other kinds of operations.

In our example we have used only 3 type of instructions:

- `mov`: copies the value in a source register to the destination register.
- `adr`: loads data from a label address to a destination register.
- `svc`: it deserves a longer explanation, check it out the next section.

## System Calls

The `svc <param>` operation, makes the processor change from User mode to Supervisor mode which gives kernel level
privileges. In our case, all supervisor calls pass `0` parameter which means it is supposed for the kernel to run a
system call. Also, values in the registers are passed as parameters to the kernel, so it knows which system call to run,
along with the required parameters for that syscall.

We are writing a program for an Apple computer, more specifically, a XNU kernel which is part of the Darwin
operating system used on macOS and iOS. All the XNU system calls can be found in the [XNU codebase][xnu_syscalls].

For our program we need to call a syscall to write data to the terminal and another to exit the program.

If we check the [write syscall][write_syscall] documentation:

```
4	AUE_NULL	ALL	{ user_ssize_t write(int fd, user_addr_t cbuf, user_size_t nbyte); }
```

The ID for the write syscall is `4` and it expects 3 arguments:

1. `fd`: which file descriptor it needs to write to. The file descriptors available are `STDIN=0`, `STDOUT=1`, and `STDERR=2` ([file descriptors][file_descriptors]).
2. `cbuf`: memory address to the buffer it needs to read from when writing to file descriptor.
3. `nbyte`: size of that buffer so it knows how much it is supposed to read from the buffer.

Therefore, based on the first section:

- `X16 = 4`: means it is calling the write syscall.
- `X0  = 1`: means it should write to the `STDOUT`.
- `X1  = helloworld`: means we are passing the memory address where the `helloworld` label data is at.
- `X2  = 15`: is the length of the hello world message.

For the second syscall, if we check the [exit syscall][exit_syscall] documentation:

```
1	AUE_EXIT	ALL	{ void exit(int rval) NO_SYSCALL_STUB; }
```

The ID for the exit syscall is `1` and it expects 1 argument:

1. `rval`: the value the process to return at the end, for example 0 for success or 1 for failure.

Therefore, based on the second section:

- `X16 = 2`: means it is calling the exit syscall.
- `X0  = 0`: means the process is ending successfully.

## Debugging

Learning how to debug an Assembly program makes development more enjoyable and simpler to understand by focusing on each
instruction individually. For more details, check out how to do it [here][debug].

## Ask for help

There were times when I couldn't solve certain issues with the resources I had available, either because I didn't know how to implement something or because I couldn't understand why something wasn't working as expected. In those cases, I asked for help on [Stack Overflow][stack_overflow] and got a great support from other developers.

Reddit also seems to have a good community, although I have not personally used it for those issues. I also tried using AI for help, but my experience Assembly related topics wasn't great. AI often mixed approaches from different CPU architectures, which don’t always work the same way. Because of this, I’d recommend being extra cautious when using AI for Assembly questions.

## References

As part of my journey learning Assembly I created [this repository][learning_assembly_arm64_apple] to keep important notes, references, and projects I wrote in that process. Check it out if you are interested in diving deeper into Assembly.

## Future Posts

I am planing to cover more complex examples of Assembly programs and other low level programming topics in future
topics, so stay tuned.

[c_lang]: https://en.wikipedia.org/wiki/C_(programming_language)
[lldb_debugger_video]: https://www.youtube.com/watch?v=v_C1cvo1biI
[debug]: https://github.com/maxclaus/learning-assembly-arm64-apple/blob/main/DEBUGGING.md
[memory_hierarchy]: https://en.wikipedia.org/wiki/Memory_hierarchy
[hard_coding]: https://en.wikipedia.org/wiki/Hard_coding
[xnu_syscalls]: https://github.com/apple-oss-distributions/xnu/blob/main/bsd/kern/syscalls.master
[file_descriptors]: https://pubs.opengroup.org/onlinepubs/9799919799/functions/stdin.html
[write_syscall]: https://github.com/apple-oss-distributions/xnu/blob/8d741a5de7ff4191bf97d57b9f54c2f6d4a15585/bsd/kern/syscalls.master#L49
[exit_syscall]: https://github.com/apple-oss-distributions/xnu/blob/8d741a5de7ff4191bf97d57b9f54c2f6d4a15585/bsd/kern/syscalls.master#L46
[stack_overflow]: https://stackoverflow.com/
[learning_assembly_arm64_apple]: https://github.com/maxclaus/learning-assembly-arm64-apple/

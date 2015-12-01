# Pexex-Lab
Program Execution Explorer lab

~~~~~~~~~~
Pexex Lab
~~~~~~~~~~
 

~~~~~~~~~~~~~~~~~~~
Part 1: GDB Trace
~~~~~~~~~~~~~~~~~~~

I typed in the first gdb command in the Eggert directory:

gdb --args ~eggert/bin64/bin/emacs-24.5 -batch -eval '(print (* 6997 -4398042316799 179))'

The program ran and exited normally saying:


[Thread debugging using libthread_db enabled]
Using host libthread_db library "/lib64/libthread_db.so.1".
[New Thread 0x7fffe3e00700 (LWP 31581)]

-896699255797638033
[Thread 0x7fffe3e00700 (LWP 31581) exited]
[Inferior 1 (process 31577) exited normally]


//The large negative number above is the output for the operation: 6997 * -4398042316799 * 179. 


I then typed typed:

(gdb) set disassemble-next-line on
(gdb) break Ftimes
(gdb) run

After running, the program first outputted:


Breakpoint 1, Ftimes (nargs=3, args=0x7fffffffd840)
    at ../../emacs-24.5/src/data.c:2767
2767	{
=> 0x00000000005438c0 <Ftimes+0>:	48 89 f2	mov    %rsi,%rdx

which tells us that Ftimes is starting with a mov function for rsi to rdx.
Because this is not exactly the format that the lab requires, I changed the format of each line to meet the requirements.
I eliminated the 3 sets of numbers in the middle of the line and added data.c:the line number, to each line. Some of the lines were lisp.h, so not all lines were data.c. 


From there, I typed:

(gdb) si 

to step into each line and function of the source code.


I typed:

(gdb) info registers

to get the values of each register after each step of si to see what got moved or changed.
By looking at the machine code, I could see which registers were being changed so I used the info registers command to find register values and used those values to fill in the right column of each line.

A couple times I tried to find the addresses of certain registers as opposed to the values, but I quickly released that was not necessary. 

Typing info registers before steping into the function of Ftimes yielded this table:


ORIGINAL REGISTERS:

rax            0xf1d440	15848512
rbx            0x7fffffffd858	140737488345176
rcx            0x400000000a000000	4611686018595160064
rdx            0x4	4
rsi            0x7fffffffd840	140737488345152
rdi            0x3	3
rbp            0x7fffffffd910	0x7fffffffd910
rsp            0x7fffffffd838	0x7fffffffd838
r8             0x2	2
r9             0x7fffffffd9f0	140737488345584
r10            0x0	0
r11            0x7ffff2068b70	140737253903216
r12            0xafa950	11512144
r13            0x180	384
r14            0x7fffffffd840	140737488345152
r15            0xbab772	12236658
rip            0x5438c0	0x5438c0 <Ftimes>
eflags         0x202	[ IF ]
cs             0x33	51
ss             0x2b	43
ds             0x0	0
es             0x0	0
fs             0x0	0




~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Part 2: Examine Integer Overflow Handling
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

I first ran the the program with optimization O2 using the command:
gcc -O2 -c testovf.c

I then used objdump in the following command to get the assembly code:
objdump -d testovf.o

I then generated all three tests using the above commands on each and recorded each assembly instructions and all the differences and similarities that I noticed. I quickly noticed that the function will overflow and return false for whenever big is greater or equal to zero. My results for these overflow handling tests are in my testovf.txt file.




~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
Part 3: A Few More Questions
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

I answered all the questions of part 3 in the answers.txt file. 

I also ran a couple of my own programs to test out how the fsanitize=undefined and -fwrapv flags changed the assembly code. 




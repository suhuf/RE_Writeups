
**Crackme.0x00:**

Hey! Let's get straight to the work!

Starting out, we are going to be using x32 debug for this EXE. In order to make finding the entry point easier you can set only the "Entry Breakpoint" to be checked under preferences:

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/9e3a281a-7add-4ec6-a9a5-8dcc0fa35846)

After that, we can see the .Exe's entry point thanks to the blue EIP indicator. When doing these exes I also like to run the exe in a separate CMD prompt on the side. 

When running the exe we see we are asked for an input, I'm going to put in "notthepassword" and see what happens!

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/03b06cdf-12c4-41f4-94fe-e2120d6832ff)

What do you know, it was invalid! Who could have thought. Now getting back to x32 debug we should start setting some breakpoints (this can be done via F2) before we analyze the EXE as it will just do the same thing we just checked without it.

Scrolling through the EXE some points seem like good candidates to set a breakpoint at:

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/062d0a42-c028-4b2f-ac70-e7109767df0f)

Specifically after and before the "Invalid password" prompt and the password ok prompt.

When looking through this area of the instruction set we notice a few things, a weird string that doesn't fit in called "250382", a command called strcmp, a test after, then a je (Jump execute) command after test eax.

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/b50a2e1d-5c64-48f1-96b3-c33e5bebe1f0)

We also see that the je (jump execute command) if it passes and executes leads to the "Password Ok" input and if not it will go to password invalid.

Let's see what happens when we put a wrong input like "1111".

When we put in 1111 we stop at our first few break points, lets press continue until we get to the JE function:

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/00961f85-cc52-4ba9-a58a-adff197984b7)

When we reach here we see in the left corner that the jump was not taken, this means that we did not return the right value that would allow us to jump to the "Password Ok" function. When we press the arrow again the EIP points to the invalid password print function and the program closes

It is clear to us that whatever is going on in that strcmp (String compare function) and test eax function is determining whether we can make the jump or not. We also can see however that the value being written to esp+4 is the number 250382

let's see what happens when we put this totally not suspicious string in:

We run the program and put in 250382 and this time we set a breakpoint at the test eax function and stop pressing when we get to the test eax function before the jump

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/97046522-4bd7-4fbc-89fd-4a0cb12cff05)

In the right corner, we can see what was written to the eax register and the value of the ZF flag which determines if we can make a jump or not, here it is a 0 (eax) and the ZF is a 1. 

When we press the arrows again we see this time it says Jump IS taken: ![image](https://github.com/suhuf/RE_Writeups/assets/105312929/6bfe1fa4-4959-41d0-8074-c14273476525)

So now we know that the value was indeed the suspicious string that was being moved to an address before strcmp (String compare) was called. Let's see what the EAX value and ZF flag value is with an incorrect password ...

We restart the program and input "1111"

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/d44b15eb-f419-43fd-b9bc-2c61491c0a71)

When we check this we see the eax this time is FFFFFFFF, this represents a -1. In addition to that, the ZF flag is not a 1. This led to the Password Invalid prompt being printed. Likewise, if you input a string the eax will have a 1 instead of a 0 & the ZF flag will have a zero. The jump won't be done.


In conclusion, we discovered that this program works by comparing the inputted string to the string "250382" and then writes to the EAX register -1 or 1 if the string is wrong or the EAX will be a 0 if correct which will allow a jump to the correct function. 

Note, this is due to the test eax eax function which changes the ZF flag if the eax is 0, the jump is more reliant on what is written to the ZF flag as a result of the function than it is to the EAX value.

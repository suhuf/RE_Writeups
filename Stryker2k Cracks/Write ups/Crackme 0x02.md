**Wassup, lets do something...**

We start off this crackme like we started the earlier versions, we plug the crack me in and we either start at the entry point due to our settings (under prefrences) or we look for a comment that says "**OptionalHeader.AddressOfEntryPoint**"

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/82f85816-67dc-4295-9cdd-466ce21a2c91)

I also like to run the exe on the side just to see what happens when we run it, so let's do that. 

We are prompted for a password let's try "notthepassword".

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/f72ddea0-d16d-42b8-8cc3-a458df12435c)

Wow it failed, can you believe that wasn't the password? 

Now let's start setting some breakpoints in our program so we can actually start doing something.

Scrolling down from the entry point we see our typical clues that we put break points around. The invalid password print section, the Password ok print section, and a Jump that determines which of the two paths the program will follow.

We also notice a similar cmp instruction that is called before the jump is executed. I am going to set a few breakpoints around this area. 


![image](https://github.com/suhuf/RE_Writeups/assets/105312929/62e62a08-02fd-439e-81cb-456ef682efa0)

So what do we find that is different from our last crackme? 

The jump that is done this time is not a je or a jae, it is a jne (Jump no execute) lets see what that means.

With some research, we can find that a jne is basically the opposite to a normal jump. In this case, if the ZF flag is a 0 the jump **WILL** be executed and if the ZF flag is a 1 the jump will **not** be executed.

Another difference we can spot is that the ebp here is being compared to the value in the eax register rather than an open apparent hexadecimal  

Old cmp instruction:
![image](https://github.com/suhuf/RE_Writeups/assets/105312929/2298b2fc-3d6f-48fb-a9d6-1f03dff9fc7f)

New cmp instruction:
![image](https://github.com/suhuf/RE_Writeups/assets/105312929/98a8af64-e235-421b-9bb9-79dc0344bfa5)


So in this case we need the ZF to be set to 1 so that the jump is NOT taken and we can reach the "Password OK" print function. And if we have the ZF to zero the jump WILL be taken and we will NOT reach our goal. 

Let's test this in real time.

We press the blue refresh button and click the arrows till we are asked for a password. Let's put "1111".

And we press the blue arrow till we get to the jne instruction call:

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/4f2ef525-0cee-4989-a9e7-282bab80e11b)


Here, if we look at the right we can see that the ZF is set to zero, but if we look on the left we can clearly see that the jump is still taken. 

We press next and then we reach the end of the program with a password invalid answer.

So, we need to get a value that matches what is being stored in the ebp register. 

An easy way to check this is when we are actually running the program, if we set a breakpoint at the cmp instruction and check the debugger's analysis we may be able to expose what value the eax register (our input in this case) is being compared too.

Let's re-run the program and put 1111 in again. Make sure you have a breakpoint at the cmp instruction if you haven't done it already. 

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/b8503c68-7d50-4adc-8c58-07cb54268d7a)

There are a few things we can see here, on the right we can see the value that is in ebp in a non truncated form, on the left under the instruction set we can also see the value of the EBP in a truncated form. In my experience the value on the bottom left is more accurate to what is needed while the one on the right is more precise.

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/c545f089-a8b0-41f8-b143-273056d1d095)

Notice here the hexadecimal value of 52B24, this is the value that is being stored in EBP-0C (EBP-0C is the same address as EBP-C).

Now we can put this into a programming calculator and convert the hexadecimal value into a proper decimal value:

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/ed6ee6c5-c9c6-458e-9c78-332a4a0755e9)

And here we have a (large) number we can use, lets put this into the program and see what happens.

We see our ZF is set to 1 once we do the compare and the jump is not taken. Thus leading to our desired password OK prompt:

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/099a7ddd-9ac0-47c2-b1a8-3af58d9ede66)



























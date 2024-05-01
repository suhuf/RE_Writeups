**Crackme.0x01:**

Hey! Let's do something...

Starting off with this crackme our EIP starts at the entry point due to the settings we set in the last challenge. If you reset the settings or don't want the settings look for this comment:  OptionalHeader.AddressOfEntryPoint

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/15ea5768-a170-4274-bb93-916d304ddfd8)


I also like to run the exe separately on another tab to see what is going on, when running we are prompted for a password. Let's try 'notthepassword' and see what happens...

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/e7f266f6-0bfa-4191-9d3d-843d64ee02a4)

Amazing, that wasn't the password! Now getting back to x32 debug we should be setting some breakpoints (done via F2) so we can actually debug some stuff.

Scrolling down in the instruction set we see a few things that look important, the Invalid password prompt, the Correct password prompt, a 'cmp command', and a je (Jump execute.) We know from our last exercise that the je commands are important, so let's put a breakpoint there.

There is also a 'cmp' command right before the jump execute and a 'scanf' function call. Let's put a breakpoint on the cmp command as well. 

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/d5b6694f-249e-4828-b05c-43ccfbaad74e)

If you remember our first crackme, you will recollect that the order we see here is very similar to the old crack me. There was a comparison (strcmp) command of the input we gave that then led to a jump that, depending on the answer we gave, either led to a 'Password Ok' or 'Password Invalid' prompt. 

Here it generally is the same except we don't have a string in the comment section shouting us the answer and this time we are using the built-in cmp instruction in assembly. So let's see what happens in the debugger when we put in a wrong password.


We press next until we get into the password input section where you can put any string you want, as for me, I'm putting "notthepassword"

We then stop at either the cmp instruction call or we stop at the je function:

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/3b2fa664-9219-44a6-9779-8114ae0c3038)

Here if you check the right hand you can see our eax was set to 0 and the ZF flag was set to 0 as well, and if you check on the left you can also see that the jump function was not taken. 

Meaning it is going to go to the invalid password print.

Now we know on an incorrect function that the ZF will have a zero. Since there is no **test eax eax** function before the jump here, unlike the last crack me, the eax value is not going to be as relevant to our reversing.

Looking back right before the jump we pay attention to the cmp instruction call and notice there are some digits and a character at the end of it. 

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/a4e0e2ab-9c9f-42c3-91f6-8e520d7c8338)

**00401365 | 817D FC 9A140000         | cmp dword ptr ss:[ebp-4],149A           |**

This means that the dword (doubleword) that is stored at ebp-4 is being compared to the value "149A", after this the jump execute command is performed or not performed according to the result and we reach the ending correct/incorrect password prints...

Aha, Surely we have solved it!!!! Let's go put in 149A and finish this.

We rerun the program with the blue refresh symbol, put in the '149A' gibberish, and find...

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/21e2576d-fec4-44a5-bfcc-1b6a99e80302)

No way, surely this must be a mistake right? Let's check the debugger.

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/038b327b-9092-406b-930c-dddd2f8e368f)

We see that the eax value is 0, not very important here since we don't have the test eax function, and we see that the ZF flag is set to zero. Why is this?

This is because the value 149A here is a hexadecimal value, it is not in the comments like the last password we found. We need to use a calculator to convert this hexadecimal into a normal value.

This is easily done with Windows' built-in calculator if you select the "programmer" option:

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/90c5a87e-7e42-4481-b9d9-e062f428fd33)

We see different values and in this case, we are going with the decimal representation, 5274.

Let's put this new number into the program and see what happens:

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/f0815445-1159-4cb1-98b6-e2deae3c690c)

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/97c66167-ebdf-490c-aaa9-2d73a67b40fb)

We see in this case 3 things, Firstly, that the EAX is not different from our wrong input, Secondly, the ZF flag is a one now, and that "the jump is taken" on the left side. Meaning we reach the 'Password Ok' print.

This means that our comparison was correct and as a result, the ZF flag was changed to 1 instead of 0. The EAX registers' value doesn't matter much here since the EAX is not being tested.

























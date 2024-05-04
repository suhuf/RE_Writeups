**Hey, let's get started!**

This crackme can be fully solved via two different methods, one longer and more complicated and the other more simple and obvious. Both of these lead to the password being cracked.

In this writeup I am going to try to explain both of these methods through **only** looking at the assembly code in x32 debug, the more complicated method however is much easier to get past using a static disassembler like Ghidra or IDA.


**The solution 1 (simpler):**

Starting off with this crackme our EIP starts at the entry point due to the settings we set in the last challenge. If you reset the settings or don't want the settings, then look for this comment: OptionalHeader.AddressOfEntryPoint

I like to run the exes on the side to see what they are about so let's do that.

We are prompted for a password, let's try something smart like 12345.

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/2f0d5f2c-e449-4b53-bbf4-26d5091b3e86)

That wasn't the password!?

Now lets get back x32dbg and start setting some breakpoints.

This time around we can see that there is a lot more things earlier on in the instruction set before we get to the password prompt function.  

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/e5b96544-c500-4291-9c65-3e62a0075ea8)

This however does not neccessitate that these things are actually being run first. It is possible that these are functions that are jumped too once the program reaches a certain point. 

In order to determine this it is much easier to put some break points in and then run the program and watch its flow of execution.

Since we know for sure that the password invalid function is going to be after the level and password prompt we can discard everything that is above 'IOLI Crackme' for now. 


![image](https://github.com/suhuf/RE_Writeups/assets/105312929/94aec4f3-dcbb-4309-9507-781336ff3fee)

We notice some actiosn are taken with our input (we notice this with the scanf function call), yet we don't see a cmp instruction called.

What we do notice is a function all before we reach the end of the program (denoted by leave). This is important as we know for sure that there needs to be a jump upwards in order for the program to choose between the valid and invalid prompts.

We click into the lower crackme0x03 function call and are sent back upwards.

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/9a6ef9c4-79e9-4670-bd9d-68b91de7751d)

Here we find two strings that appear to be obfuscated, a cmp instruction call to eax with ebp+C and a call to another function close to the strings. We also can tell it is the same address for both calls so we can just click on one.

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/77ee3465-228b-4493-8d22-08952b369181)

Through some basic reasoning, we can infer that this is most likely the deobfuscation function that is being used on one of the two strings that is passed in. This is also given away by the <JMP.&printf> function at the end. 

Howevever for this solution we don't exactly need to deobfuscate the strings nor do we need to even understand the function much. Rather all we need to do is identify one of the strings as invalid and then we can check the comparison workflow for the correct string and reach it.

So now that we know what is going on we need to set our breakpoints that we said we would a long time ago.

Scrolling back down to the obfuscated strings I am going to set a break point on these areas:

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/4329558d-580c-4362-974b-da41a7ccf9bf)

Then at the deobfuscation portion I am going to set a breakpoint on leave so it does not close too quickly.

Now lets run the program in x32 with an incorrect password.














**Solution 2 (more detailed):**

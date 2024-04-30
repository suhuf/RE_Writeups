**Crackme.0x01:**

Hey! Let's do something...

Starting off with this crackme our EIP starts at the entry point due to the settings we set in the last challenge. If you reset the settings or don't want the settings look for this comment:  OptionalHeader.AddressOfEntryPoint

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/15ea5768-a170-4274-bb93-916d304ddfd8)


I also like to run the exe sperarely on another tab to see what is going on, when running we are prompted for a password. Lets try 'notthepassword' and see what happens...

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/e7f266f6-0bfa-4191-9d3d-843d64ee02a4)

Amazing, that wasn't the password! Now getting back to x32 debug we should be setting some breakpoints (done via F2) so we can actually debug some stuff.

Scrolling down in the instruction set we see a few things that look important, the Invalid password prompt, the Correct password prompt, a 'cmp command' and a je (Jump execute.) We know from our last exercise that the je commands are important, so lets put a breakpoint there.

There is also a 'cmp' command right before the jump execute and a 'scanf' function call. Let's put a breakpoint on the cmp command aswell. 

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/d5b6694f-249e-4828-b05c-43ccfbaad74e)

If you remember our first crackme, you will recollect that the order we see here is very similar to the old crack me. There was is a comparison (strcmp) command of the input we gave that then lead to a jump that, depending on the answer we gave, either lead to a 'Password Ok' or 'Password Invalid' prompt. 

Here it generally is the same except we dont have a string in the comment section shouting us the answer and this time we are using the built in cmp instruction in assembly.

-Need to sleep 4 a bit so leaving this here for now-




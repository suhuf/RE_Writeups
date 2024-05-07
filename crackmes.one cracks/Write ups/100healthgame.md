**Hey! Let's get started.**

Today we are going to be reversing/hacking a simple program from crackmes.one called **100HealthGame** by **Aryan123**. (https://crackmes.one/crackme/65ae9d45eef082e477ff5f98)

Let's take a look at what our challenge is for this crackme:

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/49ce1c2f-c337-44e2-8dc4-8ba2bf3b50cc)

So here our challenge is to hack the program so that our health will increase by 10 every time the enter key is pressed. Let's try running the program ourselves and seeing what it's about.

In case you are also doing this exercise, the zip file password is crackmes.one as is the case with most challenges on crackmes.one.

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/9d9d550b-8099-4feb-b490-ad0e01ec625d)

We see that we press enter decreasing our health by 10 until we reach the game over portion of the segment where our health has reached zero.

Let's go into x32dbg and see what we can do.

We should start at the entry point if we have the correct settings set as specified in earlier writeups. If not we can just scroll down to the 'OptionalHeader.AddressOfEntryPoint' section of the file.

From here there are a lot of instructions we can ignore as they are not relevant to our challenge, we scroll down until we see something we recognize.


![image](https://github.com/suhuf/RE_Writeups/assets/105312929/bb6381d7-35df-443f-8407-a78c68f6427b)

Here we see the crux of the program. One of the main things we have learned to look for are **cmp** instruction calls when reading assembly, so let's take note of these ones. I am going to be using breakpoints to highlight these areas as it is quicker but you can also use the built-in highlight mode using H or right-clicking highlight mode.

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/43754675-0a81-43e6-8343-8ba9719f2af0)

Now looking at the area around the first CMP instruction we see a jump, a **sub** instruction, and then another comparison next to a jump further down.

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/63412211-9d88-4c32-94d6-188bfbc25617)

let's take a quick look at the sub instruction call: **sub dword ptr ss:[esp+1C],A** 

In assembly the sub-instruction call, as the name suggests, subtracts the value stored in an address. In this case, we see that the value at esp+1c (the top of the stack) is being subtracted by 'A'. This is a hexadecimal value. We can calculate what A means in decimal terms via a programming calculator. 

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/f815e3c9-437c-45fd-b641-de106842f311)

We see that A is equal to 10 here, meaning, the value at esp+1c is being subtracted by 10. This is the instruction that is making our program subtract 10 health every time we press enter.

We take note of that and look at the area of the last CMP instruction call.

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/ab49fb9e-bc95-44e3-92f6-02790d9c00a1)

**cmp dword ptr ss:[esp+1C],0** This should be pretty easy to understand now, 0 in hexadecimal is also equal to 0 in decimal so we don't need to convert anything.

The value in esp+1c is being compared to 0 here. 

It is important to note that when these cmp functions are done they have a direct effect on the jumps called after it. In this case, there is a jne (jump no execute) which means if the comparison returns negative then the jump IS performed but if the jump is positive the jump is NOT performed.

We also can see that the jump is to the area we just analyzed:

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/b702ff4a-e98b-4cb0-9bf8-126225972fe3)

In summary, the value in esp+1 is subtracted by 10. If after this subtraction the value is not equal to 0 the program loops back to the beginning subtraction area. If it is equal to 0 we reach this "Game over!" print call and the program exits.

**Patching:**

So all we have to do in this case is change the subtraction function call to something else, this can be done very easily in x32dbg. You just click on the address you want to change, press spacebar, change the desired instruction, and then press OK.

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/b085b32d-c5b7-42d2-a1b2-02c28ca4f20d)

We change this to add and press ok.

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/ee8d6339-1730-441f-ae59-e72f388350be)

Now let's run the program in x32dbg and see what happens.

We press the arrow when we need to pass a breakpoint and press enter when prompted.

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/3530be7d-97ca-43b4-b7e9-18ec895505a8)

We now have solved the challenge, the health now increases by 10 whenever we press enter.

However, there are still a few problems. First, the program now has no end since the value never reaches 0 thus never reaching the game over jump. With our new knowledge of how to patch instructions, we should be able to fix this easily.

This time we click on the last cmp instruction call and press spacebar.

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/4e6d693c-ad83-4143-b1d7-9262384c5ed3)

Since we know that the value at the end is being compared to the value in esp, we just need to edit that value at the end to whatever we desire. The value needs to be changed to its hexadecimal form.

In my case, I am going to use 120. Let's see what 120 is in hexadecimal form 

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/2baf7596-0435-495a-b4f8-b7ca1aa77125)

78. So we just need to input the value as 0x78 instead of 0x00.

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/ba14768e-b029-4888-8993-dfbc4d57fdb3)

Now we can press ok and see that the value has been changed.

Now is the second last task which is changing the ending string, that "Health depleted " section isn't very accurate to our new program. We can change this as well.

We right-click on the address that stores the string and then hover over 'follow in dump':

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/a32c5a60-5dfa-45fd-a34a-998c7809a9ea)

We select the bottom choice and then take a look at our new window in Dump 1.

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/914e8b9e-6e22-4c1f-b0d6-f24b9fa8cc4b)

And we take special notice of the Game Over string in the ASCII dump.

Now we need to highlight the Game Over health depleted string, We hold click until we have highlighted everything we want to edit and then right-click.

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/b9f71648-846b-428f-ba20-534b69db9d7f)

We select **Binary** and then edit till we find this window.

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/eaabfd4c-ce29-4339-93f3-ede295d8c94f)

Now you can edit the string into whatever you want in the ASCII or UTF portion. Both will respond to any edits. As for me, I am going to put 'Game over, But you win!'

We click OK and now see that the string has changed. 

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/c2161bdf-96c4-419c-a791-b1ac82430f01)

If there are any leftover letters you can write over them by putting 0s in the hex section, Just make sure not to get too close to the .@.Mingw string as this will affect the program and possibly make it not run.

**Last task:**

Now we just patch the exe, this can be done by pressing ctrl P or selecting a patch.

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/e208fddc-d0eb-46fd-bbd2-915b386da17a)

We are keeping all of our changes, now we just choose a file name and press save.

Now let's see what the program does.

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/f15bc5b9-ecdb-4ab2-aca5-d5f095ab73e7)

And it looks like our task is completed.

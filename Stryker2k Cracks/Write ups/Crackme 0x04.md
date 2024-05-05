Marhaban, let's get started.

For this crack me we load the exe into x32 debug, I also like to run the exes on the side to see what it does so let's do that. 

We are prompted for a password... Let's try 'notthepassword'!

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/1ece7bcc-954a-49b3-95fd-7a7270820098)

Our password has been graded invalid, astonishing. 

Now let's begin setting some breakpoints and looking at the program.

We start at the entry point due to our settings, or if you don't have the settings you look for a portion that is labeled 'OptionalHeader.AddressOfEntryPoint'.

Once there, we scroll down and see that large portion of instructions are being called before we get to the password request and crackme display. This is very similar to our last crackme where a function was called that jumped up to a higher set of instructions.

In this one we can set a few breakpoints around these areas even though for this crackme we are going to be using the 'Step over' tool more.

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/099e793b-a28b-471b-afa2-2378e3a5fdd2)

Scrolling down we see the typical order that we are used too with the password prompt and other aspects here are some other places you can set breakpoints for if you so wish:

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/b500c364-639a-434a-b6ac-56f8849198ed)

TBC-





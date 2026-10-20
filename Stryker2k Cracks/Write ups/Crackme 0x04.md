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

Let's select the restart button on the program and click the forward arrow until it reaches a stop:

<img width="683" height="59" alt="image" src="https://github.com/user-attachments/assets/c956f367-b393-451b-863a-7063b7e81a79" />


Let's use "notpassword" as our test password:

<img width="1096" height="160" alt="image" src="https://github.com/user-attachments/assets/072dcbe8-5669-4405-904d-3714ea84e644" />

After adding this when we check x32dbg we can see our provided string being loaded into the stack:

<img width="955" height="521" alt="image" src="https://github.com/user-attachments/assets/964f4663-4faa-4f07-a4b1-80d634968c12" />

lets continue till we hit one of our breakpoints


Here at this section we should pay a lot more attention:


<img width="1192" height="416" alt="image" src="https://github.com/user-attachments/assets/435159f6-f606-4246-a2ab-ba78566d7ff6" />

Here we can see comparisons and other steps taking place

<img width="162" height="42" alt="image" src="https://github.com/user-attachments/assets/aa245823-5f79-4f43-ace9-b2c124ddd570" />

Here we can find a JMP to a well-known function called STRLEN. With some rudimentary research we can see that see this function is used to calculate the length of a string

Since a function is being called, it is obligatory that we have some parameters that are being passed. We are aware that the string is being passed as a paramter but we should understand how:

Before a function is called, the parameter is moved to the top of the stack and then passed into the function

















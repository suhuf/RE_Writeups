**Draft**

Hey guys whats happening! 

This writeup is going to be a bit of a tangent from my other writeups and act as more of a guide than anything else. Here I am going to try to write a simple guide on how to modify and install custom software on the Quest 2 Headset.

Before this I would like to preface that there are **a lot** of steps and **a lot** of things that can go wrong when doing this during preparation phase. As a result I refer you top the other guides on the internet that are much more descriptive than this one and more professionally

maintained. The reason why this is being written on my github is that this excercise includes using digital forensics/reverse engineering tools and also helps you get more used to interacting with actual hardware and .Apk files. Without further ado let's try introducing some essential tools.


Depending on what is being modified, a game on Quest 2 or other software, we will be using different tools. Here I am going to list all of the tools that I have typically used despite some of them note being as relevant


**Tools:**

**Sidequest** (Link): (Agnostic and practically a must have for this excercise)    This is a very well made sideloader and file manager for the Quest2. With it you can backup, modify access, manage, and extract a plethroa different files within the Quest2 headset on your PC. This 

tool is functionally a must have. You can follow the the isntallation instructions on their website (//c)

**AssetStudio** (Link) (For unity compiled software, mostly games): This tool is an asset viewer/extracter that makes modifying Unity compiled games much easier. It is used to analyze cooked .asset files and is able to read bundled files 
For Unity compiled games, this and UABE (Unity Asset Bundle Extractor) are a must. This tool itself however does have its limitations. Of the most glaring of these is that it will **not** replace assets on its own. For that UABE is neccessary.

**UABE** (Link) (For unity compiled software, mostly games): To keep it simple, it is basically asset studio but with much more functionality and a less intuitive UI. It takes a lot more effort to find specific assets with this tool but It has much more functionality in extracting, replacing, and even directly editing assets.
I reccomend this fork of UABE as the original UABE development is dead. And I also reccomend trying UABENEXT3 for some functionality that is not present in the stable version. (//c)

**DNSpy**  (Link) (Agnostic, useful for everything): This tool is a must have and is probably one of my favorite tools (if only I was good with it). This tool is an intuitive well organized .DLL analyzer that will allow you to easily read, search, edit, and replace .DLL files.

The tool is used for reverse engineering software and others and is functionally a must have for Unity compiled games.


**APK Extraction process:**

words


**APK editing process:**

words

**Uncovering .DLLs with Il2cpdumper:**

words



(This should be at the end)

**APK signing process:**

The software and games on the Quest 2 headset the .APK File format. This is a very well known file format and as a result modifying software on the quest is surprisingly easy. However, despite the ease in modifiying software, getting the quest to accept the software after said modifications

is where there are a lot more steps.

Firstly, we are going to need a few tools

A compression tool like **7zip** or **Winrar**

**uber apk signer** (https://github.com/patrickfav/uber-apk-signer?tab=readme-ov-file) and its respective dependencies like **jdk8**.

Now getting to the repackaging process.

In essence, all you have to do to the extracted files from your respective .APK is merely re-zip them via either WinRAR or 7zip, with just two exceptions. 

The first of the exceptions is the Meta-inf file if you can see one in your extracted .APK directory. This will need to be **deleted** before signing as when we sign the APK a new Meta-inf folder is created.

Second exception, the resources.arsc file. Due to a requirement on Android R (version 11, API level 30) this file excpliciitly **must not** be compressed when we are remaking our APK. If this file is compressed the installation will fail and we will get this error:

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/3560dafc-71bb-46fd-b616-8872646b0bd8)

To do this via the command line in 7zip we first:

Open 7zip

Select the file and click add for the resouce.arsc file now in the popup box **make sure** these are selected:

Archive format: zip

Compression level: 0 - Store

And if encrypt file names is checked uncheck it

Now in the archive section choose the file name with the .zip extension at the end then press ok.

After that you should see a new zip file with the respective name you have chosen, click that in th 7zip gui.

Now going back to the decompiled apk directory copy all of the files **except** the resources.arsc file into the new zip file via 7zip. Make sure you are selecting each file/folder indvidually and not moving the entire directory eg:

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/50b28987-9a0d-4dae-a51d-d89ca3e87a54)


![image](https://github.com/suhuf/RE_Writeups/assets/105312929/d34b19cc-945c-46ad-a3a4-580660b78c76)




(To be updated and proof read)


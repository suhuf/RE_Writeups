**Draft**

Hey guys whats happening! 

This writeup is going to be a bit of a tangent from my other writeups and act as more of a guide than anything else. Here I am going to try to write a simple guide on how to modify and install custom software on the Quest 2 Headset.

Before this I would like to preface that there are **a lot** of steps and **a lot** of things that can go wrong when doing this during preparation phase. As a result I refer you top the other guides on the internet that are much more descriptive than this one and more professionally

maintained. The reason why this is being written on my github is that this excercise includes using digital forensics/reverse engineering tools and also helps you get more used to interacting with actual hardware and .Apk files. Without further ado let's try introducing some essential tools.


Depending on what is being modified, a game on Quest 2 or other software, we will be using different tools. Here I am going to list all of the tools that I have typically used despite some of them note being as relevant


**Tools:**

**Sidequest**: (Agnostic and practically a must have for this excercise)    This is a very well made sideloader and file manager for the Quest2. With it you can backup, modify access, manage, and extract a plethroa different files within the Quest2 headset on your PC. This also provides an easy way to send ADB commands to your headset **Note:** Make sure you are using the **Advanced** installer when installing or else you are going to miss out on a lot of functionality

tool is functionally a must have. You can follow the the isntallation instructions on their website (//c)

**AssetStudio** ([Link](https://github.com/Perfare/AssetStudio)) (For unity compiled software, mostly games): This tool is an asset viewer/extracter that makes modifying Unity compiled games much easier. It is used to analyze cooked .asset files and is able to read bundled files 
For Unity compiled games, this and UABE (Unity Asset Bundle Extractor) are a must. This tool itself however does have its limitations. Of the most glaring of these is that it will **not** replace assets on its own. For that UABE is neccessary.

**UABEA** ([Link](https://github.com/nesrak1/UABEA)) (For unity compiled software, mostly games): To keep it simple, it is basically Asset Studio but with much more functionality and a less intuitive UI. It takes a lot more effort to find specific assets with this tool but It has much more functionality in extracting, replacing, and even directly editing assets.
I reccomend this fork of UABE as the original UABE seems to be inactive. And I also reccomend trying UABEANEXT3 for some functionality that is not present in the stable version. (//c)

**DNSpy**  (Link) (Agnostic, useful for everything): This tool is a must have and is probably one of my favorite tools (if only I was good with it). This tool is an intuitive well organized .DLL analyzer that will allow you to easily read, search, edit, and replace .DLL files.

The tool is used for reverse engineering software and others and is functionally a must have for Unity compiled games.


**APK Extraction process:**

The APK extraction process is very simple with sidequest, however the setup to get it working is a far more difficult endeavour. As a basic run down you first need to enable developer mode on your VR headset **prior** to being able to do anything with sidequest, you then need too follow the respective instructions for installing sidequest on the headset and etc. This entire process goes way beyond the scope of this guide/writeup so we are going to focus on the extraction process in this section.

First thing, go into to settings on sidequest and scroll down to 'Sidequest stuff' and click on 

Assuming everything else has already been set up, turn on the headset and enter the respective password if you have one and then plug it in to the PC. 


![image](https://github.com/suhuf/RE_Writeups/assets/105312929/813e6748-75d9-44f1-b597-7659b780493e)

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/a3be2773-0ccb-47cf-b52c-359f978968e1)

You then go on currently installed apps:

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/5587543f-e7f2-4460-bda0-0ed0ed23542a)

And select the gear icon on the respective app according to its name, lets say in this case I want to extract the APK of virtual desktop:

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/ecf6819e-75f8-4b66-bd07-5bbc623a9e67)

Now a new popup should appear, When you are ready to backup press "Backup APK File" and then after that click the gear icon again and press open backups

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/bb47f106-7d1e-4b04-8ee9-098fbe8b11a0)

In the APK folder there should be an APK file of your desired software.

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/02a0d928-46f9-489b-9b62-680479519f73)

Now that we have extracted it, how do we access it? The good thing .Apk files is that they are extracted the same way as zip files, just rename the file's extension to .zip and then extract the contents.


![image](https://github.com/suhuf/RE_Writeups/assets/105312929/4b5a6b04-fafa-4f04-a37a-4a7e3df8e1b0)

Now we have all of the .APKs contents and you can start changing whatever you like.



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

And if encrypt file names is checked, uncheck it.

Now in the archive section choose the file name with the .zip extension at the end then press ok.

After that you should see a new zip file with the respective name you have chosen, click that in th 7zip gui.

Now going back to the decompiled apk directory copy all of the files **except** the resources.arsc file into the new zip file via 7zip. Make sure you are selecting each file/folder indvidually and not moving the entire directory eg:

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/50b28987-9a0d-4dae-a51d-d89ca3e87a54)


![image](https://github.com/suhuf/RE_Writeups/assets/105312929/d34b19cc-945c-46ad-a3a4-580660b78c76)

Now if everything worked properly there should be a loading screen that is compressing the new assets you have added into your respective .zip file. It will have the name you have chosen for it earlier.

Now in order to 'convert' the .zip file into a .apk you just need to rename the file's extension from .zip to .apk.

And now you have packaged your new modded apk for the Quest 2!  ... .....not really. Here is where the hard stuff starts happening.


As of now, the Quest 2 obligates that all .APKs are signed via the **v2 android apk signing scheme** if the apk is not signed via this the .apk will not install on to the quest. You can even test this yourself if you want via Sidequest via the apk installation instructions here: (**link something here)**



**Side Note:**

In the event you do try the installation you will get an error that looks like this on sidequest: ![image](https://github.com/suhuf/RE_Writeups/assets/105312929/a1871e5d-ebf5-43ca-b4db-0808ba31517f)



"A task failed. Check the tasks screen for more info. 2024-05-19T06-08-25.093Z_72.apk: /data/local/tmp/_stream.apk could not be installed [INSTALL_PARSE_FAILED_NO_CERTIFICATES: Failed collecting certificates 

for /data/app/vmdl231055552.tmp/base.apk: Failed to collect certificates from /data/app/vmdl231055552.tmp/base.apk: META-INF/BS.SF indicates /data/app/vmdl231055552.tmp/base.apk is signed using APK Signature Scheme v2, but no such signature was found. Signature stripped?]"

What it is saying is that it needs the .apk to be signed via the v2 scheme and it isnt.


So what we need to do is sign and also align the new .apk, if you are already familiar with android development and are able to do this via your own method I reccomend doing that. I however was not familiar with this process so I tried to use the quickest method I could think of.

Here is where we are going to use **uber-apk-signer**, this will speed up the process of signing the the new modded apk and re-aligning it. Follow the installation instructions here: (https://github.com/patrickfav/uber-apk-signer?tab=readme-ov-file).

Once you have the tool installed **and its dependencies installed** we canmove to the first step of signing which is generating a keystore. **And yes, as of 2024 you do need to generate and use a keystore when signing .apks for the quest 2**, the default debug keystore does not work in my experience and generates an error.


For this we will be using keytool which is already installed with the JDK development kit, a dependencies of uber apk signer. If you are unsure if you have this installed, open a CMD prompt and type in java. If it returns this you're good.

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/bceaf36c-3cd5-4577-80b8-1c9999e2bf11)


The keytool exe is located in the bin directory of your Java installtion, generally speaking it should be this: **C:\Program Files\Java\jdk-<version>\bin** and within there you should find a file named keytool. If not, you are going to have to check the JDK's installation path.

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/193d42c3-509b-4604-8916-ac14dd9cd767)

Now in this directory we run the cmd prompt and put in this command

**keytool.exe -genkeypair -v -keystore my-release-key.jks -keyalg RSA -keysize 2048 -validity 10000 -alias** **my-key-alias**

you can change the my-release-key.jks to a better name if you so like.

change the my-key-alias to whatever you can remember, this is going to be needed in the future.

You are then going to be asked for a password (make this something you can remember as we are going to need it in the future) and some annoying questions, you can skip the questions via pressing enter but you are going to need to answer atleast one question.

It should look similar to this:

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/623c504b-d381-46b5-a7ad-f02a617990cb)


After this a file will be saved as <chosenname>.jks in the folder that we made it in unless you specified another folder.

Now that we have a key we can sign the apk. 

We are going to use **uber-apk-signer** which will both sign and align the apk quickly, this is the command we will be using:

**java -jar uber-apk-signer-1.3.0.jar --ks "C:\Program Files\Java\jdk-21\bin\my-release-key.jks" --apks "<Path to your apk>"**

It should ask for a password and possibly an alias, In the future I will probably be adding a visual here **(//c)**

And with that a new apk should be made that is signed with v2 and compatible with Quest.



**Installing Modded APK to Quest:**

Judging by the fact you have made it this far, I am assuming you are now a bit familiar with how sidequest works, for installing the new apk to Quest 2 we make sure the headset is connected and select "**Install APK file from folder on computer**"


![image](https://github.com/suhuf/RE_Writeups/assets/105312929/943e6028-0329-4bff-9ac6-cb000afb634c)

Select in the respective diretory the **SIGNED and Aligned** .apk file with whatever name you have chosen or the default name if default.

**Note:**

With this the app should be installed, in the event there are errors here are a few errors that can occur and their respective solutions:

**
[-124: Failed parse during installPackageLI: Targeting R+ (version 30 and above) 



requires the resources.arsc of installed APKs to be stored uncompressed and aligned on a 4-byte boundary]:**

This error is caused by either the alignment being off or resource.arsc being compressed with the other parts of the apk, to fix the resource file issue follow the steps above. For a basic rundown, you click add with the resource.arsc file without any compression and then copy the other files into the new zip file.

You can fix the other error by checking the .apks alignment **zipalign -c -v 4 application.apk** you may need to resign the apk after doing this.
 

[INSTALL_PARSE_FAILED_NO_CERTIFICATES: Failed collecting certificates 



for /data/app/vmdl231055552.tmp/base.apk: Failed to collect certificates from /data/app/vmdl231055552.tmp/base.apk: META-INF/BS.SF indicates /data/app/vmdl231055552.tmp/base.apk is signed using APK Signature Scheme v2, but no such signature was found. Signature stripped?]:

This error is caused due to the APK not being signed. Quest 2 Does not allow the installation of .APKs without signatures, this also is caused due to the original META-INF folder not being deleted. Solution is to delete the old META-INF directory and then sign the apk with its new signature, outlined above. 

Important to note that even if the apk is signed with the default debug key it will not be accepted for installtion, a person key needs to be made.


**Checking APK against blacklist...**:

This is an error that is caused via sidequest's protections and not ADB or Quest Restrictions. Sidequest uses this as an anti-piracy measure and blocks .apks that are known to be pirated. Regardless of personal opinions on this, I ran into this error when trying to mod an APK without internet. If the APK does not pass this blacklist the sidequest will refuse to install, this however proves a problem when you do not have internet as there is not a local copy of the blacklsit downloaded on the system.


Regardless can be bypassed by using ADB directly. Either use ADB on your own system or use sidequest's custom ADB commands tool (it's really all the same).

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/d15c06a3-ac39-4983-a92b-5bfa85d97478)


![image](https://github.com/suhuf/RE_Writeups/assets/105312929/09c2c703-cde8-481d-b72f-420be85319e0)

Here you enter:

**adb install -g "Full APK path"**

And you should be able to install without the blacklist check. 































(To be updated and proof read)


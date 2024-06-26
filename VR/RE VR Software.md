**Draft**

Hey, guys what's happening! 

This write-up is going to be a bit of a tangent from my other writeups and act as more of a guide than anything else. Here, I am going to try to write a simple guide on how to modify and install custom software on the Quest 2 Headset.

Before this, I would like to preface that there are **a lot** of steps and **a lot** of things that can go wrong during this process during the preparation phase. As a result, I refer you to check the other guides available on the internet that are much more descriptive than this one and more professionally maintained. 

I am including this on my Github as the exercise includes using some digital forensics & reverse engineering tools and also helps you get more used to interacting with actual hardware and APK files. Without further ado let's try introducing some essential tools.

The tools we do and do not use are very reliant on the specific app we are editing, here I am just going to list the tools that I have most commonly used when modifying on the Quest 2. It is possible all of these tools or none of these tools are optimal for the specific software you want to hack into.

**Tools:**

**Sidequest**: (Agnostic and practically a must-have for this exercise)    This is a side-loader and file manager for the Quest 2 with an easy-to-understand UI. With it, you can backup, modify access, manage, and extract different files within the Quest2 headset on your PC. This also provides an easy way to send ADB commands to your headset **Note:** Make sure you are using the **Advanced** installer when installing or else you are going to miss out on a lot of functionality.
 Installation instructions are present on their website.

**AssetStudio** ([Link](https://github.com/Perfare/AssetStudio)) (For Unity compiled software, mostly games): This tool is an asset viewer/extractor that makes modifying Unity compiled games much easier. It is used to analyze cooked .asset files and can read bundled files 
For Unity-compiled games, this and UABE (Unity Asset Bundle Extractor) or UABEA (Updated fork of UABE)are a must. This tool itself however does have its limitations. Of the most glaring of these is that it will **not** replace assets on its own. For that UABE/UABEA is necessary.

**UABEA** ([Link](https://github.com/nesrak1/UABEA)) (For unity compiled software, mostly games): To keep it simple, this is basically Asset Studio but with much more functionality and a bit more cluttered UI. It takes a lot more effort to find specific assets with this tool but it has much more functionality in extracting, replacing, and even directly editing assets.
I recommend this fork of UABE (UABEA )as the original UABE seems to be inactive. I also recommend trying UABEANEXT3 for some functionality that is not present in the stable version. ([Link](https://github.com/nesrak1/UABEANext/tree/master))

**DNSpy**  (Link) (Agnostic, useful for everything): This tool is a must-have and is probably one of my favorite tools (if only I was good with it). This tool is an intuitive well organized .DLL analyzer that will allow you to easily read, search, edit, and replace .DLL files.

The tool is used for reverse engineering software and is functionally a must-have for Unity games compiled via Mono.

**Il2cppDumper** (Link):
Information


**APK Extraction process:**

The APK extraction process is very simple with Sidequest, however, the setup to get it working can be a far more difficult endeavor. As a basic run down, you will first need to enable developer mode on your VR headset **prior** to being able to do anything with Sidequest, you then need to follow the respective instructions for installing Sidequest on the headset and etcetera. This entire process goes way beyond the scope of this guide/writeup (which is already 300 lines!) so we are going to focus on the extraction process in this section.

Assuming everything else has already been set up, turn on the headset enter the respective password if you have one, and then plug it into the PC. 

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/813e6748-75d9-44f1-b597-7659b780493e)

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/a3be2773-0ccb-47cf-b52c-359f978968e1)

You then go on currently installed apps:

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/cd131fe7-d445-4608-9f2c-eb20313ac575)

Select the gear icon on the respective app according to its name, let's say in this case I want to extract the APK of Virtual desktop:

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/ecf6819e-75f8-4b66-bd07-5bbc623a9e67)

Now a new popup should appear, When you are ready to backup press "Backup APK File" and then after that click the gear icon again and press open backups

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/bb47f106-7d1e-4b04-8ee9-098fbe8b11a0)

In the APK folder, there should be an APK file of your desired software.

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/02a0d928-46f9-489b-9b62-680479519f73)

Now that we have extracted it, how do we access it? The good thing about APK files is that they are extracted the same way as zip files, just rename the file's extension to .zip and then extract the contents via 7zip or another tool.

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/4b5a6b04-fafa-4f04-a37a-4a7e3df8e1b0)

Now we have all of the APK contents and you can start changing whatever you like.

**APK editing process:**

The APK editing process is highly reliant on how the software was compiled. For unity-built games, for example, we would use the previously mentioned tools such as **AssetStudio**, **UABE**, and **DNSpy**. Especially **DNSpy** if it was compiled via mono.

For other software, our editing processes might be a bit different. For example, maybe the software we are working with is highly reliant on .DLLs and has an .Exe that interacts with said .DLLs. In this case, we may just use DNSpy alone with x64/x32Dbg to try and uncover what we can change. In this scenario, I am going to go through a bit of the editing process for Unity compiled software. As the majority of what you will be modding will most likely be similar.

**First step, Locate .assetfiles:**

For things compiled with unity, .asset files are a core component of the ecosystem. Asset files can contain **Textures, Models, Audio Clips, Scripts** and more. As a result, editing these files is essential for making the modifications we want. 

Depending on how the developer of the App made their architecture, it may take a bit of searching to find these **.asset** files. An easy way to check if a file is an asset file right off the bat is by just putting it into Asset Studio and checking if it accepts it.

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/3373bfec-cd7c-4e03-a756-fd7381a0e703)

This is the APK directory of a game that has a large amount of these files, you can see here how the most obvious asset file should look like; "**Sharedassets1.assets"**. However, this game also has other asset files that do not follow this obvious naming scheme.

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/b3d3449d-50e0-4095-8995-d651c7a0bc12)

Here, for example, we see a large number of files that are suspiciously in the same directory as the earlier .asset file. 

One giveaway that these are also asset files is that they also have a .resource copy. We can put this into Asset Studio can confirm for ourselves:

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/63b82ee8-bec2-42b3-9a8c-861e001cd917)


![image](https://github.com/suhuf/RE_Writeups/assets/105312929/43032d81-e14e-40df-b7c5-5475f73481eb)

We can see here that this file named "f8805b958f82b224d82bdc4b5a50f20a" with no extension actually was an asset file and that it actually is storing over 2k assets. 

We can also investigate the assets themselves if they are renderable by Asset Studio. 'Texture 2d' assets, for example, are renderable by Asset Studio. You can filter your result to only contain a specific asset via the drop-down menu.

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/512a0a03-ca8b-4589-bd92-d69465cb86a9)

An example here is the Unity logo,

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/d28e06e8-bd2c-4a05-8393-d6292dd03c96)

Another example with an asset that is specific to the app. Now the question occurs, how can we edit these assets? For each type of asset, there is a different process in how we are going to have to deal with it, for example editing a game object/model is going to be a lot different from a texture or audio clip. In this case, we are doing Texture2d which is relatively simple to edit.

We simply extract the asset, edit it with paint.net (or something else), and then re-import it in via **UABEA/UABE** (Asset studio cannot replace assets itself.)

Let's test this by making the unity asset orange or something. We right-click on the desired asset and click export ![image](https://github.com/suhuf/RE_Writeups/assets/105312929/1d801854-044e-4fa3-8eb3-06268654f960)

And then we choose a directory and open the file in Paint or whichever other software.

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/75ff837c-acfe-4215-901a-d4c844e9425f)

(Pretty lame ngl) 

Now that we have finished editing this, we can save it and re-import it in the future. 

The next step is getting the source file name via Asset Studio, for more organized APKs this step can be skipped as everything should be easy to remember but the APK I am using in this case is not as organized. 

Right-click on the desired asset and choose 'Show original File'.


![image](https://github.com/suhuf/RE_Writeups/assets/105312929/01c4da96-30da-4ca6-bbaf-9352a1c3f1e9)

It will then highlight the file source for you to check. Take note of the file name or keep the file in hand because we are going to be accessing it with a new tool

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/d913f063-0dd1-4808-9cdd-02576f7f5a8c)

Now we can either open UABEA/UABE and directly drag it into the app or we can manually input the directory path via File --> Open

Now the asset should load.

**Important note:** The asset cannot be accessed via AssetStudio and UABEA at the same time, if a new asset is being loaded via UABE while AssetStudio is reading the asset you are going to encounter an error and have to close UABEA before being able to do anything. This can lead to a lot of progress being lost

 IE: When loading an asset with UABEA **CLOSE** AssetStudio first.

 Here we select the asset and click on plugins:

 ![image](https://github.com/suhuf/RE_Writeups/assets/105312929/f9e97e05-3a75-42af-ae26-29da6de9639d)

Go to edit texture, click Ok, then click on "Load" right next to texture. Now we click save and we also need to click save again for the actual file to save the newly loaded asset, or we can use CTRL-S. Now let's look into AssetStudio and see if it worked.

 ![image](https://github.com/suhuf/RE_Writeups/assets/105312929/f5099e36-e112-418c-8f07-33fb78e2ab01)

 ![image](https://github.com/suhuf/RE_Writeups/assets/105312929/32f9adeb-82a3-4aeb-bbd8-7757f54b4e23)

 ![image](https://github.com/suhuf/RE_Writeups/assets/105312929/b12bb6f3-360f-4986-97db-01ac0d70524a)

Yep, it worked. And we can also see that the sprite also changed when we changed the Texture2d. Now whenever this is rendered by the App in the headset it will show this new image.


This is basically how the less complicated parts of the modding process go, Asset is extracted via AssetStudio, it is edited via some software (depending on asset type) and then the new asset is imported via UABEA. 

This however is an oversimplification, for things like audio files for example, there is more work that is done in the editing process such as converting a .wav/mp3 file to a .fsb file which is then imported. But overall, it fits into this workflow.


The next section should cover how you can actually read and change some of the code of these apps.

**Uncovering Dummy .DLLs with Il2cpdumper:**

words


(This should be at the end)



**APK signing process:**

The software and games on the Quest 2 use the APK File format. This is a very well-known file format and as a result, modifying software on the quest is surprisingly easy. However, despite the ease of modifying software, getting the quest to accept the software after said modifications is where there are a lot more steps.

Firstly, we are going to need a few tools

A compression tool like **7zip** or **Winrar**

**uber apk signer** (https://github.com/patrickfav/uber-apk-signer?tab=readme-ov-file) and its respective dependencies like **jdk8**.

Now getting to the repackaging process.

In essence, all you have to do to the extracted files from your respective APK is merely re-zip them via either WinRAR or 7zip, with just two exceptions. 

The first of the exceptions is the Meta-inf directory if you can see one in your extracted APK directory. This will need to be **deleted** before signing as when we sign the APK a new Meta-inf folder is created.

The second exception; the resources.arsc file. Due to a requirement on Android R (version 11, API level 30) this file explicitly **must not** be compressed when we are remaking our APK. If this file is compressed the installation will fail and we will get this error:

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/3560dafc-71bb-46fd-b616-8872646b0bd8)

To do this via the command line in 7zip we first:

Open 7zip

Select the file and click Add for the resource.arsc file. 

Now in the popup box **make sure** these are selected:

Archive format: zip

Compression level: 0 - Store

And if encrypt file names is checked, uncheck it.

Now in the archive section choose the file name with the .zip extension at the end then press ok.

After that you should see a new zip file with the respective name you have chosen, click that in the 7zip gui.

Now going back to the decompiled APK directory, copy all of the files **except** the resources.arsc file into the new zip file via 7zip. Make sure you are selecting each file/folder individually and not moving the entire directory eg:

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/50b28987-9a0d-4dae-a51d-d89ca3e87a54)


![image](https://github.com/suhuf/RE_Writeups/assets/105312929/d34b19cc-945c-46ad-a3a4-580660b78c76)

Now if everything worked properly there should be a loading screen that is compressing the new assets you have added into your respective .zip file. It will have whatever name you chose for it earlier.

Now to 'convert' the .zip file into a .apk you just need to rename the file's extension from .zip to .apk.

And now you have packaged your new modded app for the Quest 2! ... except not really. Here is where the hard stuff starts happening.


As of now, the Quest 2 obligates that all APKs are signed via the **v2 android APK signing scheme** if the APK is not signed via this the APK will not install on to the quest. 

You can even test this yourself if you want to by skipping to the installation instructions at the end.



**Side Note:**

In the event you do try the installation you will get an error that looks like this on sidequest: ![image](https://github.com/suhuf/RE_Writeups/assets/105312929/a1871e5d-ebf5-43ca-b4db-0808ba31517f)



"A task failed. Check the tasks screen for more info. 2024-05-19T06-08-25.093Z_72.apk: /data/local/tmp/_stream.apk could not be installed [INSTALL_PARSE_FAILED_NO_CERTIFICATES: Failed collecting certificates 

for /data/app/vmdl231055552.tmp/base.apk: Failed to collect certificates from /data/app/vmdl231055552.tmp/base.apk: META-INF/BS.SF indicates /data/app/vmdl231055552.tmp/base.apk is signed using APK Signature Scheme v2, but no such signature was found. Signature stripped?]"

What it is saying is that it needs the APK to be signed via the v2 scheme and it isn't currently signed.

So what we need to do is sign and also align the new APK, if you are already familiar with Android development and can do this via your own method I would recommend doing that. 

I however was not familiar with this process so I tried to use the quickest method I find.

Here is where we are going to use **uber-apk-signer**, this will speed up the process of signing the new modded APK and re-aligning it. Follow the installation instructions here: (https://github.com/patrickfav/uber-apk-signer?tab=readme-ov-file).

Once you have the tool **and its dependencies installed** we can move to the first step of signing which is generating a keystore. **And yes, as of 2024 you do need to generate and use a keystore when signing .apks for the Quest 2**, the default debug keystore does not work in my experience and generates an error.


For this, we will be using keytool which is already installed with the JDK development kit, a dependency of uber APK signer. If you are unsure if you have this installed, open a CMD prompt and type in Java. If it returns this you should be good.

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/bceaf36c-3cd5-4577-80b8-1c9999e2bf11)


The keytool exe is located in the bin directory of your Java installation, generally speaking, it should be this: **C:\Program Files\Java\jdk-<version>\bin**, and within there you should find a file named keytool. If not, you are going to have to check the JDK's installation path.

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/193d42c3-509b-4604-8916-ac14dd9cd767)

Now in this directory, we run the cmd prompt and put in this command

**keytool.exe -genkeypair -v -keystore my-release-key.jks -keyalg RSA -keysize 2048 -validity 10000 -alias** **my-key-alias**

You can change the my-release-key.jks to a better name if you so wish

You also should change the my-key-alias name to whatever you can remember, this is going to be needed in the future.

You are then going to be asked for a password (make this something you can remember as you are going to need it in the future) and some annoying questions, you can skip the questions by pressing enter but you are going to need to answer at least one question.

It should look similar to this:

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/623c504b-d381-46b5-a7ad-f02a617990cb)


After this, a file will be saved as <chosenname>.jks in the folder that we made it in unless you specify another folder.

Now that we have a key we can sign the APK.

We are going to use **uber-apk-signer** which will both sign and align the APK quickly, this is the command we will be using:

-Run with administrator-

**java -jar uber-apk-signer-1.3.0.jar --ks "C:\Program Files\Java\jdk-21\bin\my-release-key.jks" --apks "<Path to your apk>"**

It should ask for a password and possibly an alias, In the future I will probably be adding a visual here **(//c)**

And with that, a new APK should be made that is signed with v2 and compatible with Quest.


**Installing Modded APK to Quest:**

Judging by the fact you have made it this far, I am assuming you are now a bit familiar with how Sidequest works, for installing the new APK to Quest 2 we make sure the headset is connected and select "**Install APK file from folder on computer**"


![image](https://github.com/suhuf/RE_Writeups/assets/105312929/943e6028-0329-4bff-9ac6-cb000afb634c)

Select in the respective directory the **SIGNED and Aligned** .APK file with whatever name you have chosen or the default name if default.

**Note:**

With this the app should be installed, in the event there are errors here are a few errors that can occur and their respective solutions:

**
[-124: Failed parse during installPackageLI: Targeting R+ (version 30 and above) 



requires the resources.arsc of installed APKs to be stored uncompressed and aligned on a 4-byte boundary]:**

This error is caused by either the alignment being off or resource.arsc being compressed with the other parts of the APK, to fix the resource file issue follow the steps above. For a basic rundown, you click add with the resource.arsc file without any compression and then copy the other files into the new zip file.

You can fix the other error by checking the .apks alignment **zipalign -c -v 4 application.apk** you may need to resign the APK after doing this.
 

[INSTALL_PARSE_FAILED_NO_CERTIFICATES: Failed collecting certificates 



for /data/app/vmdl231055552.tmp/base.apk: Failed to collect certificates from /data/app/vmdl231055552.tmp/base.apk: META-INF/BS.SF indicates /data/app/vmdl231055552.tmp/base.apk is signed using APK Signature Scheme v2, but no such signature was found. Signature stripped?]:

This error is caused due to the APK not being signed. Quest 2 Does not allow the installation of APKs without signatures, this also could be caused due to the original META-INF folder not being deleted. 

The solution is to delete the old META-INF directory and then sign the APK with its new signature, as outlined above. 

Important to note that even if the APK is signed with the default debug key it will not be accepted for installation, a personal key needs to be made.


**Checking APK against blacklist...**:

This is an error that is caused via Sidequest's protections and not by ADB or Quest Restrictions. Sidequest uses this as an independent third-party anti-piracy measure and blocks APKs that are known to be pirated. Regardless of personal opinions on this, I ran into this error when trying to mod an APK without the internet. If the APK does not pass this blacklist the Sidequest will refuse to install said APK, this however proves a problem when you do not have internet as there is not a local copy of the blacklist downloaded to the system.


Regardless can be bypassed by using ADB directly. Either use ADB on your own system or use Sidequest's custom ADB commands tool (it's really all the same).

![image](https://github.com/suhuf/RE_Writeups/assets/105312929/d15c06a3-ac39-4983-a92b-5bfa85d97478)


![image](https://github.com/suhuf/RE_Writeups/assets/105312929/09c2c703-cde8-481d-b72f-420be85319e0)

Here you enter:

**adb install -g "Full APK path"**

And you should be able to install without the blacklist check. 



























(To be updated and proofread)


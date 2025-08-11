

Hello everyone, In this tutorial I will be covering how to edit strings in Unity compiled games/software. This is essential for mods that consist of making **custom translations**, **new dialogue**, **descriptions**, and others. 

**Why is this is the VR directory?**

The majority of VR software is Unity compiled and as a result editing them is reliant on understanding this. Regardless, this should work for any Unity compiled product aslong as they have used a similar method to store strings.

**Why wasn't this added to the VR modding tutorial?**

I actually was stuck trying to find out how to this for months on end, analyzing .DLLs in mono compilied games and trying to understand what was actually being read by the program so I could edit it. It was only extremely recently that I found a tutorial that made it clear how you can do this for a fully compiled Unity project.

That tangent aside, the previous VR modding tutorial is already 300+ lines long and I feel already exceeds its original scope.  

**Preface:** The issue of editing dialogue and strings is highly reliant on the methodolgy of the developer. Some developers will have a file named 'localization' or similar that will host all of the visible strings in the game in different languages. Others may have a proprietary file type that will require a custom made tool to decrypt or read. 

I am making this guide on the basis that the developer is **not** doing that and is instead using Unity's typical built in functionality which makes finding these lines take a lot more effort.


Necessary tools:

**UABEA ()**

**Good string searcher (using grep)**

**Possibly asset studio(?)**

incomplete




grep -ri "String" <Desired Directory> --text

# kittyware.dll
![Image](https://cdn.discordapp.com/attachments/1261466827968745492/1277691700244123721/d1772bf5-b2ea-468f-9b6a-a208e1beaa9c.jpg?ex=66ce169f&is=66ccc51f&hm=387f1b13f0552d6118889a8be8017f2da3c80b07278e11620af7b11a0cef5345&)

this is one of my open source malwares
First of all we will add a new empty PE section, given a file path, we read the entire file, check its DOS signature for a valid PE, check if it is a x86 executable (important since we will only inject x86 opcode into this section), check if the section we wanna create doesn't already exist and if all checks out,we create a new section with a given size and characteristics.Comments are in the code and it is better explained there.
This all is done by the AddSection function.It is good to mention that the method explained here uses the file on the disk,without mapping it into memory and applying read/write operations there.It highly relies on file pointers.

The second and the toughest part is adding the code into the newly created section.
So,basecaly what i want to do to prove this concept is this:

    Add a new section
    Retrive and save the original entry point from the Optional Header (the address from witch the actual program starts)
    We change the OEP to point to our new section,so when the user opens the executable,the program starts at our code,does whatever we want BEFORE any other original code,then returns to its original entry point so the program will run as usual! In this case,i will pop a message box with the string "Hacked" in it!

    Add a new section
    Retrive and save the original entry point from the Obtional Header (the address from witch the actual program starts)
    We change the OEP to point to our new section,so when the user opens the executable,the program starts at our code,does whatever we want BEFORE any other original code,then returns to its original entry point so the program will run as usual! In this case,i will pop a message box with the string "Hacked" in it!

IMPORTANT
Since the loader loads kernel32 at a different address each time the computer is restarted,we will need to DYNAMICALY retrive its base address,from PEB,so we can look for a function in the export table named LoadLibraryA, call it with "hearteyes.dll" as argument,and later retrieve the function address of MessageBoxA from user32.dll using a call to GetProcAddress.

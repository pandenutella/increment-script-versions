# increment-script-versions

Increment script versions via commandline using batch

##### Before

```
./scripts/a.sql
./scripts/b_V2.sql
./scripts/c_V3.sql
```

##### After

```
./scripts/a_V2.sql
./scripts/b_V3.sql
./scripts/c_V4.sql
```

## Instructions:

1. Create batch file `${filename}.bat` inside `scripts` folder. Use one word only for the filename to make it easier for
   you to call the command in the commandline. In below examples, I'll be using `mybat` as the filename. 
   
   Try typing `echo Hi` in the batch file and run it by opening the `scripts` folder in the terminal and entering the
   batch filename without the `.bat` extension _(For example, run `mybat.bat` by entering `mybat` in the terminal)_.
   Your terminal should look like below example after running the batch file.
   ```
   your-path\..\scripts>mybat
  
   your-path\..\scripts>echo Hi
   Hi
   ```
   In the above example, we learned that `echo` is used to print messages in the terminal.
2. When creating our own batch scripts, most of the time, we wouldn't want to display the commands that are being
   executed; What we probably want to see are logs and output only. **Search for a way to hide the commands executed**
   and achieve below example. 
   
   _Hint: The command involves the word `echo`._
   ```
   your-path\..\scripts>mybat
   Hi
   ```
   Now that we know how to hide the executed commands, let's now remove the `echo Hi` from our script and proceed with
   the next steps.
3. To increment the version of the scripts inside a folder, we must first know how to display them in the terminal. **This
   can be achieved by using the `dir` command.** Running it in the terminal should show below result.
   ```
   Volume in drive ...
   Volume Serial Number is ...
   
   Directory of your-path\..\scripts
   
   DD/MM/YYYY HH:mm am     <DIR>       .
   DD/MM/YYYY HH:mm am     <DIR>       ..
   DD/MM/YYYY HH:mm am             22  a.sql
   DD/MM/YYYY HH:mm am             22  b.sql
   DD/MM/YYYY HH:mm am             22  c.sql
   DD/MM/YYYY HH:mm am             9   mybat.bat
             4 File(s)             75 bytes
             2 Dir (s) 93,042,683,904 bytes free
   ```
   We were now able to display all the files and folders inside the `scripts` folder, including other information like
   drive volume, serial number, file creation date, total file sizes, etc.
4. Since we will only be needing the filenames of the scripts to increment their version, we should limit the display to
   filenames only. **Search for a way to make the `dir` command show filenames only** and achieve below result.
   ```
   a.sql
   b_V2.sql
   c_V3.sql
   mybat.bat
   ```
5. We were now able to display the names of the files in `scripts` folder, but it's still displaying our batch file. We
   should exclude it in the results since we only want to increment the versions of `.sql` files. **Search for a way to
   achieve below result.**
   ```
   a.sql
   b_V2.sql
   c_V3.sql
   ```
6. Let us first increment the version of the scripts with existing versions before renaming non-versioned scripts to
   version 2. **Search for a way to further filter the results to only display `.sql` files with _V[0-9] suffix** and
   achieve below result.

   _Hint: You will be piping `|` another command with `dir` and that command should filter the `dir` output using
   regular expression_
   ```
   b_V2.sql
   c_V3.sql
   ```
   **Put the same commands that you used in your batch file and run it to show the same result.**
7. Before we proceed, **let's first extract the `.sql` extension that we used in our commands to a variable**, since we
   will be using it many times later. We can create a variable using `set variableName=variableValue` and read variable
   using `%variableName%`. After extracting `.sql` to a variable, the batch file should still work as expected.
8. To be able to increment the version of each script that we displayed using our current commands, we should capture
   each filename first to perform actions to them individually. We can achieve this by using `for loops`. **Try
   integrating the code below in your file filtering commands**:
   ```
   for /f %%i in ('<<your filter commands here>>') do (
       echo %%i
   )
   ```
   When you run your batch file, you will most likely encounter `| was unexpected at this time`. **Try fixing it by
   searching for the behavior of pipes `|` inside `for loops`**. Once fixed, it should show below result.
   ```
   b_V2.sql
   c_V3.sql
   ```
9. Let's first set the filename to a variable since we will be calling it multiple times later. Running the script now should show below result.
   ```
   c_V3.sql
   c_V3.sql
   ```
   Here, it displayed `c_V3.sql` twice, but we were expecting it to display both `b_V2.sql` then this. This happened because of batch's default variable expansion behavior. We can fix this by changing the variable expansion mode to `delayed`. **This can be achieved by calling `setlocal enabledelayedexpansion` before the for loop and changing the syntax of dynamic variable calls inside the `for loop` from `%variableName%` to `!variableName!`** as below.
   ```
   ...
   setlocal enabledelayedexpansion
   
   for /f ...
      ...
      echo !filename!
   ...
   ```
   After enabling delayed expansion and changing the variable call syntax, the script should now again display below result.
   ```
   b_V2.sql
   c_V3.sql
   ```
10. We now need to capture the filenames without their `.sql` extension and their versions. **Search for a way to read `for loop` variables without extensions and also how to substring variables.** **Set both the filename (without extension) and its current version to separate variables then echo them.** Your script should show below result. 
   ```
   b_V2
   2
   c_V3
   3
   ```
11. Now that we've captured the current versions, we can now increment them to the next version. Try adding 1 to the current version when setting the version variable then verify. What you did will probably result to string concatenation instead of addition as below.
   ```
   b_V2
   2 + 1
   c_V3
   3 + 1
   ```
   **Search for a way to make the `set` command treat the given variable value as an arithmetic expression and achieve below result.**
   ```
   b_V2
   3
   c_V3
   4
   ```
12. **Remove the separate echos for filename and version** and **replace it with a single `echo`, displaying the concatenated filename substring, version, and extension** to achieve below result.
   ```
   b_V3.sql
   c_V4.sql
   ```
13. Now that we have the next filename of the versioned `.sql` scripts, we should now rename them. **Search for a way to rename files using batch** and achieve below result.
   
   Terminal:   
   ```
   your-path\..\scripts>mybat
   ```
   scripts folder:
   ```
   a.sql
   b_V3.sql
   c_V4.sql
   mybat.bat
   ```
   We have now incremented the versions of versioned scripts.
14. Next, we'll now increment the version of non-versioned scripts _(a.sql)_. **Make a copy of the `for loop` for versioned scripts and apply necessary changes to make it filter files that does not match the given pattern and concatenate the filename and the version correctly.** Apply these changes and achieve below result. 
    
   Terminal:
   ```
   your-path\..\scripts>mybat
   ```
   scripts folder:
   ```
   a_V2.sql
   b_V4.sql
   c_V5.sql
   mybat.bat
   ```
15. To make this script usable from anywhere in our PC, we should **place it in a folder and add that folder's path to the PATH environment variable**. In my case, I'll place it in `D:\custom-commands\`. After doing these steps, you should now be able to call your batch file from anywhere in your PC. 
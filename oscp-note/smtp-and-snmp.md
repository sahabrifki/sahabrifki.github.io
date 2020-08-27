# SMTP and SNMP

SMTP Enumeration - Mail Severs

 - Verify SMTP port using Netcat  
 \`nc -nv $ip 25\`

- POP3 Enumeration - Reading other peoples mail - You may find usernames and passwords for email accounts, so here is how to check the mail using Telnet

 root@kali:~\# telnet $ip 110  
 +OK beta POP3 server \(JAMES POP3 Server 2.3.2\) ready  
 USER billydean  
 +OK  
 PASS password  
 +OK Welcome billydean

 list

 +OK 2 1807  
 1 786  
 2 1021

 retr 1

 +OK Message follows  
 From: jamesbrown@motown.com  
 Dear Billy Dean,

 Here is your login for remote desktop ... try not to forget it this time!  
 username: billydean  
 password: PA$$W0RD!Z

SNMP Enumeration -Simple Network Management Protocol

 - Fix SNMP output values so they are human readable  
 \`apt-get install snmp-mibs-downloader download-mibs \`  
 \`echo "" &gt; /etc/snmp/snmp.conf\`

 - SNMP Enumeration Commands

 - \`snmpcheck -t $ip -c public\`

 - \`snmpwalk -c public -v1 $ip 1\|\`

 - \`grep hrSWRunName\|cut -d\\* \\* -f\`

 - \`snmpenum -t $ip\`

 - \`onesixtyone -c names -i hosts\`

 - SNMPv3 Enumeration  
 \`nmap -sV -p 161 --script=snmp-info $ip/24\`

 - Automate the username enumeration process for SNMPv3:  
 \`apt-get install snmp snmp-mibs-downloader \`  
 \`wget [https://raw.githubusercontent.com/raesene/TestingScripts/master/snmpv3enum.rb\`](https://raw.githubusercontent.com/raesene/TestingScripts/master/snmpv3enum.rb%60)

 - SNMP Default Credentials  
 /usr/share/metasploit-framework/data/wordlists/snmp\\_default\\_pass.txt


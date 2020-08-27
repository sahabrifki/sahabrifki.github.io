# DNS enum

- DNS Enumeration

 - NMAP DNS Hostnames Lookup  
 \`nmap -F --dns-server &lt;dns server ip&gt; &lt;target ip range&gt;\`

 - Host Lookup  
 \`host -t ns megacorpone.com\`

 - Reverse Lookup Brute Force - find domains in the same range  
 \`for ip in $\(seq 155 190\);do host 50.7.67.$ip;done \|grep -v "not found"\`

 - Perform DNS IP Lookup  
 \`dig a domain-name-here.com @nameserver\`

 - Perform MX Record Lookup  
 \`dig mx domain-name-here.com @nameserver\`

 - Perform Zone Transfer with DIG  
 \`dig axfr domain-name-here.com @nameserver\`

 - DNS Zone Transfers  
 Windows DNS zone transfer

 \`nslookup -&gt; set type=any -&gt; ls -d blah.com \`

 Linux DNS zone transfer

 \`dig axfr blah.com @ns1.blah.com\`

 - Dnsrecon DNS Brute Force  
 \`dnsrecon -d TARGET -D /usr/share/wordlists/dnsmap.txt -t std --xml ouput.xml\`

 - Dnsrecon DNS List of megacorp  
 \`dnsrecon -d megacorpone.com -t axfr\`

 - DNSEnum  
 \`dnsenum zonetransfer.me\`

- NMap Enumeration Script List:

 - NMap Discovery  
 \[\*[https://nmap.org/nsedoc/categories/discovery.html\*\]\(https://nmap.org/nsedoc/categories/discovery.html\)](https://nmap.org/nsedoc/categories/discovery.html*]%28https://nmap.org/nsedoc/categories/discovery.html%29)

 - Nmap port version detection MAXIMUM power  
 \`nmap -vvv -A --reason --script="+\(safe or default\) and not broadcast" -p &lt;port&gt; &lt;host&gt;\`


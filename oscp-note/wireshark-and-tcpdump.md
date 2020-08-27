# wireshark & tcpdump

Wireshark  
 - Show only SMTP \(port 25\) and ICMP traffic:

 \`tcp.port eq 25 or icmp\`

 - Show only traffic in the LAN \(192.168.x.x\), between workstations and servers -- no Internet:

 \`ip.src==192.168.0.0/16 and ip.dst==192.168.0.0/16\`

 - Filter by a protocol \( e.g. SIP \) and filter out unwanted IPs:

 \`ip.src != xxx.xxx.xxx.xxx && ip.dst != xxx.xxx.xxx.xxx && sip\`

 - Some commands are equal

 \`ip.addr == xxx.xxx.xxx.xxx\`

 Equals

 \`ip.src == xxx.xxx.xxx.xxx or ip.dst == xxx.xxx.xxx.xxx \`

 \` ip.addr != xxx.xxx.xxx.xxx\`

 Equals

 \`ip.src != xxx.xxx.xxx.xxx or ip.dst != xxx.xxx.xxx.xxx\`

Tcpdump

 - Display a pcap file  
 \`tcpdump -r passwordz.pcap\`

 - Display ips and filter and sort  
 \`tcpdump -n -r passwordz.pcap \| awk -F" " '{print $3}' \| sort -u \| head\`

 - Grab a packet capture on port 80  
 \`tcpdump tcp port 80 -w output.pcap -i eth0\`

 - Check for ACK or PSH flag set in a TCP packet  
 \`tcpdump -A -n 'tcp\[13\] = 24' -r passwordz.pcap\`


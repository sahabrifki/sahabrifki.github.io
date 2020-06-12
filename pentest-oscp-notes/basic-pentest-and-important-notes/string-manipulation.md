# string manipulation

- Count number of lines in file  
 \`wc -l index.html\`

 - Get the start or end of a file  
 \`head index.html\`

 \`tail index.html\`

 - Extract all the lines that contain a string  
 \`grep "href=" index.html\`

 - Cut a string by a delimiter, filter results then sort  
 \`grep "href=" index.html \| cut -d "/" -f 3 \| grep "\\." \| cut -d '"' -f 1 \| sort -u\`

 - Using Grep and regular expressions and output to a file  
 \`cat index.html \| grep -o '[http://\\[^"\\]\\*'](http://\[^) \| cut -d "/" -f 3 \| sort –u &gt; list.txt\`

 - Use a bash loop to find the IP address behind each host  
 \`for url in $\(cat list.txt\); do host $url; done\`

 - Collect all the IP Addresses from a log file and sort by  
 frequency  
 \`cat access.log \| cut -d " " -f 1 \| sort \| uniq -c \| sort -urn\`


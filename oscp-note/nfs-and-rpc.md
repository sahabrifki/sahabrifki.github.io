# NFS & RPC

- NFS \(Network File System\) Enumeration

 - Show Mountable NFS Shares  
 \`nmap -sV --script=nfs-showmount $ip\`

RPC \(Remote Procedure Call\) Enumeration

 - Connect to an RPC share without a username and password and enumerate privledges  
 \`rpcclient --user="" --command=enumprivs -N $ip\`

 - Connect to an RPC share with a username and enumerate privledges  
 \`rpcclient --user="&lt;Username&gt;" --command=enumprivs $ip\`


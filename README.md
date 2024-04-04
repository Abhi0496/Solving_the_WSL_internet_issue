# Solving_the_WSL_internet_issue
Steps I took to solve the WSL internet issue on my system

When using Ubuntu(WSL) in backend and VS Code in front end we might face the issue of not able to install the 3rd party libraries because sometimes WSL doesn't really have access to the internet. I was facing the same issue and found the solution that worked for me

Step 1) I opened WSL and tried to ping google by-  
```ping google.com``` and Alternatively, I tried pinging a known IP address like 1.1.1.1 (Cloudflare's DNS server) ```ping 1.1.1.1``` and got the below output
```
abhi@IMS-BX22ND3:/mnt/c/Users/015229175/Documents/wordle$ ping google.com
ping: google.com: Temporary failure in name resolution
abhi@IMS-BX22ND3:/mnt/c/Users/015229175/Documents/wordle$ ping 1.1.1.1
PING 1.1.1.1 (1.1.1.1) 56(84) bytes of data.
64 bytes from 1.1.1.1: icmp_seq=1 ttl=54 time=25.2 ms
64 bytes from 1.1.1.1: icmp_seq=2 ttl=54 time=19.0 ms
64 bytes from 1.1.1.1: icmp_seq=3 ttl=54 time=35.6 ms
64 bytes from 1.1.1.1: icmp_seq=4 ttl=54 time=18.4 ms
64 bytes from 1.1.1.1: icmp_seq=5 ttl=54 time=20.0 ms
```

Now WSL was unable to resolve the domain name google.com to an IP address, which is likely due to a DNS issue. However when I tried to ping IP address, it was successfull which meant that WSL was able to send and receive packets over internet using IP address.
Based on above observation, it can be said that issue might be related to DNS resolution within WSL environment.

Step 2) Next thing I did was to update the DNS settings. In order to do that we have to first do ```cd``` then ```cd /etc``` finally ```sudo nano resolv.conf``` to access the file. The file had ```nameserver 172.24.0.1``` by default which was not working. I commented it out and then added ```nameserver 8.8.8.8 
nameserver 8.8.4.4``` which is a public DNS server of google. Saved it by pressing ```ctrl+x then y```

![image](https://github.com/Abhi0496/Solving_the_WSL_internet_issue/assets/23584147/63812418-6a8c-45a9-9fb6-2aeec6bac7d0)

Step 3) Next step was to shut down and restart the WSL using the following commands-
open run by pressing windows + r and then type wsl --shutdown and then again restart the WSL by simply opening it again. But I found out that my resolv.conf file was generating again by default with default values so I skipped step 3 (will probably find the solution to it later)

Later I just again did ```ping google.com``` and got the required result
```
root@IMS-BX22ND3:~# ping google.com
PING google.com (142.251.46.238) 56(84) bytes of data.
64 bytes from sfo03s27-in-f14.1e100.net (142.251.46.238): icmp_seq=1 ttl=115 time=19.1 ms
64 bytes from sfo03s27-in-f14.1e100.net (142.251.46.238): icmp_seq=2 ttl=115 time=21.1 ms
64 bytes from sfo03s27-in-f14.1e100.net (142.251.46.238): icmp_seq=3 ttl=115 time=13.9 ms
64 bytes from sfo03s27-in-f14.1e100.net (142.251.46.238): icmp_seq=4 ttl=115 time=17.6 ms
```


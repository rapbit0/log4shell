# log4shell

I have created lists of hosts known to exploit CVE-2021-44228. These .csv files are ready for import to CheckPoint Management via mgmt_cli.

These  lists are based on the following sources:

gnremy_hosts.csv : https://gist.github.com/gnremy/c546c7911d5f876f263309d7161a7217#file-cve-2021-44228_ips-csv
blotus_hosts.csv : https://gist.github.com/blotus/f87ed46718bfdc634c9081110d243166#file-log4j_exploitation_attempts_crowdsec-csv

The files are setup like the following:

name,ipv4-address,groups,color,tags
h_167.99.88.151_CVE-2021-44228,167.99.88.151,groupname,red,CVE-2021-44228_Log4J
h_167.99.44.32_CVE-2021-44228,167.99.44.32,groupname,red,CVE-2021-44228_Log4J
h_167.99.36.245_CVE-2021-44228,167.99.36.245,groupname,red,CVE-2021-44228_Log4J
h_167.71.4.81_CVE-2021-44228,167.71.4.81,groupname,red,CVE-2021-44228_Log4J


name: h_167.99.88.151_CVE-2021-44228  // consists of h_IPADDRESS_CVE-2021-44228
ipv4-address: IPv4Address
groups: groupname                     //The group you want to add the hosts to
color: red
tags: CVE-2021-44228_Log4J


How To:

1. First create the Network Group you want to add the hosts to on your Management and publish the changes. You can also create the Group "groupname", and change the Group Name after the hosts have been added to the group. If you dont use "groupname" as a group, make sure to edit the .csv file!

2. Run mgmt_cli your prefered way with the command "add host --batch filename.csv". See 3. if you are not sure how to use mgmt_cli

3. Connect to your Management Server via ssh
4. Copy blotus_hosts.csv & gnremy_hosts.csv to the management server (via sftp,scp, etc.)
5. In expert mode run the command: dos2unix blotus_hosts.csv         (dos2unix will convert the files from DOS line endings (carriage return + line feed) to Unix line endings (line feed). This is was necessary for me after editing the files in excel)
6. In expert mode run the command: dos2unix gnremy_hosts.csv

**7. (For Single Domain)**
// login save 'session-id' into text file called id.txt
mgmt_cli login user admin password vpn123  > id.txt 

// use the id.txt as a file from which the session-id (your token) is taken and perform add host command.
mgmt_cli add host --batch blotus_hosts.csv -s id.txt
mgmt_cli add host --batch gnremy_hosts.csv -s id.txt

// publish and logout (again using the same session-id)
mgmt_cli publish –s id.txt
mgmt_cli logout –s id.txt

**7. (For Multi Domain)**
// login to domain named MyDomain and save 'session-id' into text file called id.txt. (you can use -d "MyDomain" as well)
mgmt_cli login user admin password vpn123 domain "MyDomain" > id.txt 

// use the id.txt as a file from which the session-id (your token) is taken and perform add host command.
mgmt_cli add host --batch blotus_hosts.csv -s id.txt
mgmt_cli add host --batch gnremy_hosts.csv -s id.txt

// publish and logout (again using the same session-id)
mgmt_cli publish –s id.txt
mgmt_cli logout –s id.txt

**7. (For the Lazy, single domain)**
mgmt_cli -r true add host --batch blotus_hosts.csv
mgmt_cli -r true add host --batch gnremy_hosts.csv

8. Alternative to publishing your session via CLI(mgmt_cli publish –s id.txt): Connect to Smart Console, goto Manage & Settings -> Sessions -> View Sessions -> Right Click on your Web API Session containing the changes -> Take Over the Session or publish. 
9. If you have used the default group "groupname", change the group to something else.
10. If you have any questions  or something isn't working, feel free to ask!
11. If there are any other IPs you need a list for, let me know and I will add it


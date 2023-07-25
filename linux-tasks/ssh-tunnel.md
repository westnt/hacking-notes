## problem
we are ssh'd into a remote server from our attack machine.

the server has a webpage on localhost:8080. we can access this page from the server, but we would like to be able to access this from the attack machine.

solution: ssh tunneing!

from the attacker machine, we can establish an ssh connection to the server and tunnel the service back to us.
```bash
attacker$ ssh -N user@server -L 8888:localhost:8080
```
this establishes ssh connection from attacker to server. The -P says forward to port 8888 on the attacker machine, the service beining served at locahost:8080 from the server.

now, from the attacker machine, we can access the service on port 8888
```bash
attacker$ wget localhost:8888
```

Note that
```bash
attacker$ ssh -N user@server -L 8888:localhost:8080
```
is shorthand for
```bash
attacker$ ssh -N user@server -L 8888:localhost:localhost:8080
```

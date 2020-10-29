In this example we are preparing ssh keys, to allow one machine to connect to others. this is for ansible

master machine = that will generate the ssh connection
ssh-keygen
```
this will generate the private key and public key in ~/.ssh
```

copy public key to slave machines
on master machine
```
gotot ~/.ssh
copy id_rsa.pub value

login slave machine
goto location ~/.ssh
copy public key from master in slave ~/.ssh/authenticated_keys
```

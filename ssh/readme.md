In this example we are preparing ssh keys, to allow one machine to connect to others. this is for ansible

master machine = 192.168.56.2
slave = 192.168.56.3

### 1.sh-keygen
```
this will generate the private key and public key in ~/.ssh
```

### 2. copy public key to slave machines
on master machine goto ~/.ssh and copy id_rsa.pub value

### 3. login slave machine
goto location ~/.ssh
copy public key from master in slave ~/.ssh/authenticated_keys

### 4. try ssh
nowfrom master machine, type below command
```
ssh vagrant@192.168.56.3
```

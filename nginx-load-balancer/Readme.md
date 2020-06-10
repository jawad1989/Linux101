# installing load balancer on NGINX

In this example we will use AWS to create 3 EC2 instances(ubuntu) 
first two instances will have apache installed on them that will host our sample website and by configuring NGINX on third machine we will use it as a load balancer

Load Balancer: Private IP: 172.31.62.104 ; Public IP: 34.207.237.138
Machine 1:     Private IP: 172.31.60.216 ; Public IP: 100.24.240.169
Machine 2:     Private IP: 172.31.58.117 ; Public IP: 34.229.56.240

## Step 1: install Apache on both machines

1. Once machines are up and running, ssh into them 
2. run `sudo apt update`
3. to install apache run `sudo apt install apache2`
    This will create a `/var/www/html` with sample `index.html' file
    ```
    /var/www/html$ ll
    total 20
    drwxr-xr-x 2 root root  4096 Jun 10 03:04 ./
    drwxr-xr-x 3 root root  4096 Jun 10 03:03 ../
    -rw-r--r-- 1 root root 10931 Jun 10 03:04 index.html
    ```
4. For `Machine 1' we will update the `index.html` heading to 
```
 <span class="floating_element" style="color:orange">
    Apache2 Slave 1 - Orange
  </span>
```
5. 4. For `Machine 2' we will update the `index.html` heading to 
```
 <span class="floating_element" style="color:pink">
    Apache2- Slave 2 Pink
  </span>
```
6. Save the files and test the changed version in browser by hitting their Public IPs

***Machine 1:***

![orange](https://github.com/jawad1989/Linux101/blob/master/images/orange.PNG)


***Machine 2:***

![orange](https://github.com/jawad1989/Linux101/blob/master/images/pink.PNG)

## Step 2: install Apache on both machines Now we will install Nginx Server and configure on Load Balancer on `Server`
1. Install Nginx server
```
sudo apt isntall nginx -y
```
2. update the conf file 
```

ubuntu@ip-172-31-62-104:~$ sudo cat /etc/nginx/nginx.conf



events {}
http {
      upstream backend {
               server 172.31.60.216:80;
               server 172.31.58.117:80;
      }

     server {
             listen 80;
             location / {
               proxy_pass http://backend;
             }
     }
}

```

3. start the nginx server
```
sudo service nginx start
```

4. Test your load balancer by hitting public Ip

The requests will use round robbin as this is the default method

***Test 1:***

![orange](https://github.com/jawad1989/Linux101/blob/master/images/LB-Orange.PNG)


*** Test 2: ***

![PinkLB](https://github.com/jawad1989/Linux101/blob/master/images/LB-Pink.PNG)

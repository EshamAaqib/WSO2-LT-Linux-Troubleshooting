# WSO2-Linux-Troubleshooting-Assignment

## Resource Limitation ( Soft and Hard limits )

### 1. Creating a user with name : user1 : Execute the following

```
sudo adduser user1
```

### 2. Setting the user1 soft limit RAM size to 1 GB : Execute the following
###### Before running the below command switch to the newly created user by executing ```sudo su - user1```

```
ulimit -Sm 1048576
```

### 3. Setting the user1 soft limit file size to 2MB and Hard Limit file size to 3MB : Execute the following

```
ulimit -Sf 2048          
ulimit -Hf 3072  
```

### 4. Setting the user1 Linux open file limit Soft = 5000 hard = 6000 : Execute the following

```
ulimit -Sn 5000 
ulimit -Hn 6000
```

### 5. Setting the servers network card’s receiving ring buffer to Maximum value

###### Before executing anything below you need to find the network interface name that you need to set the ring buffer. To do so execute ```ifconfig``` and note down the name.

###### Then execute the following to find the maximum ring buffer values. ens33 is the nic name.

```
ethtool -g ens33
```

###### Then finally execute the following to set the max value. The max value on mine is 4096

```
ethtool -G ens33 rx 4096 
```

### 6. Screenshots

###### Screenshot of ulimit -Ha

![image](https://user-images.githubusercontent.com/75664650/124746796-e3569200-df3e-11eb-8e75-5813d1a240db.png)

###### Screenshot of ulimit -Sa

![image](https://user-images.githubusercontent.com/75664650/124746895-04b77e00-df3f-11eb-81b3-991a583c0db7.png)

###### Screenshot of  /etc/security/limits.conf

![image](https://user-images.githubusercontent.com/75664650/124746985-1a2ca800-df3f-11eb-9e8d-2be91ecc860f.png)




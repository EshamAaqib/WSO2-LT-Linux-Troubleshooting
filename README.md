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

## SWAP

### Creating Two swap files with size of 100MB and 200MB

```
dd if=/dev/zero of=/swap bs=100M count=1
dd if=/dev/zero of=/swap1 bs=200M count=1 
mkswap /swap
mkswap /swap1 
swapon /swap
swapon /swap1
```

### Assigning 200MB swap file a priority 10 and 100MB swap file a priority 20

```
swapoff /swap
swapon -p 20 /swap
swapoff /swap1
swapon -p 10 /swap1
```

### Configuring swap files to mount during a boot

###### In this step I went ahead and added these two lines to the end of the /etc/fstab file. Use ```sudo nano /etc/fstab``` to edit it.

```
/swapfile swap swap pri=20 20 20
/swapfile1 swap swap pri=10 10 10
```

### Changing the system swappiness value to 40

```
sudo sysctl vm.swappiness=40
```

###### To make the swappiness persistent across reboots I went ahead and added the following to the /etc/sysctl.conf file.

```
vm.swappiness=40
```

### Screenshots

###### Screenshot of /etc/fstab

![image](https://user-images.githubusercontent.com/75664650/124748634-fc604280-df40-11eb-85e7-915d5be8497a.png)

###### Screenshot of Free -m

![image](https://user-images.githubusercontent.com/75664650/124748712-126e0300-df41-11eb-86e1-3a7e2331247e.png)

###### Screenshot of Swapon -s

![image](https://user-images.githubusercontent.com/75664650/124748776-24e83c80-df41-11eb-93cd-67ecd48fd316.png)

## FTP

### Configuring and setting up the proftpd FTP server

###### Installing proftpd

```
sudo apt-get install -y proftpd
```
###### Configuring Server name. I went ahead and editted the ServerName field in the /etc/proftpd/proftpd.conf file.

### Creating user proftpuser

###### Switch to root by typing ```sudo su -``` and execute the following :

```
sudo echo "/bin/false" >> /etc/shells
```

###### Then run the following to create the user and set the password :

```
sudo useradd -m -s /bin/false proftpuser
sudo passwd proftpuser
```
###### After running the above you can exit root by typing ```exit```

### Seting the proftpd process priority ( Nice value) as -5

```
renice -n -5 -p $(pgrep ^proftpd$)
```

### Assigning the I/O priority of Constant to the proftpd process

```
ionice -c 2 -n 0 proftpd
```

### Screenshots

###### proftpd config file has been uploaded to the config files folder

###### Screenshot of top

![top](https://user-images.githubusercontent.com/75664650/124750336-f5d2ca80-df42-11eb-9a0f-6bf875a2e7d6.png)





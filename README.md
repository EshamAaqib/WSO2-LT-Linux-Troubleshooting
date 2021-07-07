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





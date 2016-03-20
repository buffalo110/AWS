# Adding Swap Space for Ubuntu Instance

EC2 instance on AWS doesn't come with swap space. It works OK most of the time, but it could run into memory issues. I had memory issues 
when I try to install 'dplyr' package for R. This tutorial walks through creating a swap file and using it as the swap space: 
https://www.digitalocean.com/community/tutorials/how-to-add-swap-on-ubuntu-14-04. 
The tutorial has more details. Here are the steps I did:

#### Check Available Space on the instance

The typical way of allocating space for swap is to create a swap file as a separate partition on the existing partition.
Check the current memory usage:

    $ df -h
The summary of the memory usage:

    Filesystem      Size  Used Avail Use% Mounted on
    udev            492M   12K  492M   1% /dev
    tmpfs           100M  340K   99M   1% /run
    /dev/xvda1      7.8G  1.7G  5.7G  24% /
    none            4.0K     0  4.0K   0% /sys/fs/cgroup
    none            5.0M     0  5.0M   0% /run/lock
    none            497M     0  497M   0% /run/shm
    none            100M     0  100M   0% /run/user

The **/dev/xvda1** indicates 5.7G available space. 1G will be allocated to a swap file. 

#### Create a Swap File

A swapfile will be created in the root directory. The file will contain the same amount of memory as the expected swap space. 

Creat a 1G file:

    $ sudo fallocate -l 1G /swapfile
Verify that the correct amount of space was reserved:

    $ ls -lh /swapfile

It shows the correct space is set aside. 

    -rw-r--r-- 1 root root 1.0G Mar  3 20:00 /swapfile

#### Enabling the Swap File

Lock down the file so it only acts as swap space. 

    $ sudo chmod 600 /swapfile

Verify the permissions:

    $ ls -lh /swapfile

Only root user is able to read and write now:

    -rw------- 1 root root 1.0G Mar  3 20:00 /swapfile

Set up the swap space:

    $ sudo mkswap /swapfile
    Setting up swapspace version 1, size = 1048572 KiB
    no label, UUID=9a2cdeb8-70de-4424-872a-8b6eece4cf39

Enable the swap space:

    $ sudo swapon /swapfile

Check if the swap space is set up successfully:

    $ sudo swapon -s
    Filename				Type		Size	Used	Priority
    /swapfile                               file		1048572	0	-1

Check free memory:

    $ free -m
                  total       used       free     shared    buffers     cached
    Mem:           992        197        794         64          3         92
    -/+ buffers/cache:        102        890
    Swap:         1023          0       1023

Now the swap space is set up successfully. Installing R packages does not trigger memory problem any more. 

---
title: How to expand available memory on your Raspberry Pi using ZRAM
date: 2016-03-28 10:43:49 +0100
---

The Raspberry Pi is a great little, cheap, low-powered computer; however the trade offs of this low power come at a price - you are unable to do a number of really useful things on it. One of the biggest limiting factors of the Raspberry Pi is the RAM - at 1GB (or even 512MB on older models), there's not a lot you can do with this. You quickly exhaust available memory when you start multitasking.

Fortunately, there's something called virtual memory which enables computers to have more memory available to applications than is available with RAM only. The virtual memory is made up of the RAM and a little portion of storage called the swap space. You can theoretically have as much as 2GB of virtual memory available to applications on your Raspberry Pi using a swap space of 1GB in size. Raspbian allocates 100MB of swap space by default and it is done this way because using a larger amount of space will result in a much larger number of I/O operations on the SD/MicroSD card. Now, due to the fact that these storage systems have limited write cycles, larger swap space will deplete the available write cycles on MicroSD card; quickly turn the MicroSD card into a read-only device.

However, ZRAM works differently; rather than using an amount of physical storage for the swap space, it uses a portion of the existing RAM to create additional swap space. It is able to do this because it compresses the contents of the swap space that maps to physical RAM essentially giving the illusion of more RAM space.

I've found ZRAM to be a more convenient option for expanding available memory for applications on the Raspberry Pi. ZRAM is installed by default on the latest version of Raspbian (based on Debian Jessie as of this writing). To enable it:

```
sudo modprobe zram
```

That's not enough, you'll still need to configure the disk size of the ZRAM device. Here's a script that allows you setup ZRAM automatically, you can modify it to set your own parameters on how large it should be.

```
#!/bin/bash
### BEGIN INIT INFO
#Provides: zram
#Required-Start:
#Required-Stop:
#Default-Start: 2 3 4 5
#Default-Stop: 0 1 6
#Short-Description: Increased Performance In Linux With zRam (Virtual Swap Compressed in RAM)
#Description: Adapted for Raspian (Rasberry pi) by eXtremeSHOK.com using https://raw.github.com/gionn/etc/master/init.d/zram
### END INIT INFO
 
start() {
    mem_total_kb=$(grep MemTotal /proc/meminfo | grep -E --only-matching '[[:digit:]]+')
 
    modprobe zram
 
    sleep 1
    #only using 50% of system memory, comment the line below to use 100% of system memory
    mem_total_kb=$((mem_total_kb/2))
 
    echo $((mem_total_kb * 1024)) > /sys/block/zram0/disksize
 
    mkswap /dev/zram0
 
    swapon -p 100 /dev/zram0
}
 
stop() {
    swapoff /dev/zram0
    sleep 1
    rmmod zram
}
 
case "$1" in
    start)
        start
        ;;
    stop)
        stop
        ;;
    restart)
        stop
        sleep 3
        start
        ;;
    *)
        echo "Usage: $0 {start|stop|restart}"
        RETVAL=1
esac
```

Simply save this file in `/etc/init.d` directory (calling it `zram`) and ensure it has the execute bit set a.k.a `sudo chmod +x zram` and then you can configure your Raspberry Pi to execute this when the Raspberry Pi is booted.

```
sudo update-rc.d zram defaults
```
If you want to activate zram without restarting the Raspberry Pi you can do so by running `sudo service zram start`.
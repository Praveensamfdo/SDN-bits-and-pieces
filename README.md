#### Changing the link speed of CPqD switch

------------

- First modify the source code of ofsoftswitch13.
```bash
$ cd ofsoftswitch13
$ gedit lib/netdev.c
```

- lib/netdev.c:
```c
        if (ecmd.autoneg) {
            netdev->curr |= OFPPF_AUTONEG;
        }

        //netdev->speed = ecmd.speed;
	netdev->speed = 1; /*Fix to 1 Mbps link*/

    } else {
        VLOG_DBG(LOG_MODULE, "ioctl(SIOCETHTOOL) failed: %s", strerror(errno));
    }
}
```
- Then, re-install ofsoftswitch13.

```bash
$ make clean
$ ./boot.sh
$ ./configure
$ make
$ sudo make install
```
Source : [RyU QoS](https://osrg.github.io/ryu-book/en/html/rest_qos.html "RyU QoS")

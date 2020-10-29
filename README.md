## Changing the link speed of CPqD switch

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

## MaxiNet installation guide

------------

- Download the installer.sh script.<br>
`wget https://raw.githubusercontent.com/MaxiNet/MaxiNet/master/installer.sh`

- Run installer.sh('yes' to all when prompted)<br>
`./installer.sh`

- Create the **MaxiNet.cfg** file in /etc and copy the following code.

```bash
; place this at ~/.MaxiNet.cfg
[all]
password = pravsam
controller = 130.127.133.100:6634
logLevel = INFO        ; Either CRITICAL, ERROR, WARNING, INFO  or DEBUG
port_ns = 9090         ; Nameserver port
port_sshd = 5345       ; Port where MaxiNet will start an ssh server on each worker
runWith1500MTU = False ; Set this to True if your physical network can not handle MTUs >1500.
useMultipleIPs = 0     ; for RSS load balancing. Set to n > 0 to use multiple IP addresses per worker. More information on this feature can be found at MaxiNets github Wiki.
deactivateTSO = True   ; Deactivate TCP-Segmentation-Offloading at the emulated hosts.
sshuser = root         ; On Debian set this to root. On ubuntu set this to user which can do passwordless sudo
usesudo = True         ; If sshuser is set to something different than root set this to True.
useSTT = False         ; enables stt usage for tunnels. Only usable with OpenVSwitch. Bandwithlimitations etc do not work on STT tunnels!

[FrontendServer]
ip = 130.127.133.83
threadpool = 256       ; increase if more workers are needed (each Worker requires 2 threads on the FrontendServer)

[ubuntu-vm1]
ip = 130.127.133.83
share = 1

[ubuntu-vm2]
ip = 130.127.133.52
share = 1
```

- Change the controller IP, password, worker IPs and front-end server IP accrodingly.

- Use the following command to start the front-end server.<br>
`screen -d -m -S MaxiNetFrontend MaxiNetFrontendServer`

- Use the following command to start a worker.<br>
`screen -d -m -S MaxiNetWorker sudo MaxiNetWorker`

Source : [MaxiNet](https://maxinet.github.io/ "MaxiNet")

## Fogbed installation guide

------------

- Install cmake.<br>
`apt-get install cmake`

- Install importlib-metadata.<br>
`pip install importlib-metadata==0.12`

- Finally install Fogbed using bare-metal installation given in the GitHub page.

Source : [Fogbed](https://github.com/fogbed/fogbed "Fogbed")

## Enable ssh access in Containernet Docker containers

------------

- Install ssh server.<br>
`apt install openssh-server`

- Add the following configurations to **/etc/ssh/sshd_config**.
```bash
PermitRootLogin yes
PasswordAuthentication yes
UseLogin yes
RSAAuthentication no
PubkeyAuthentication no
```
- Restart the **ssh** service.<br>
`service ssh restart`

- Change the root password.<br>
`passwd root`

## Using reinforcement learning in a Mininet environment

------------

- It is not required to setup an external SDN controller, when using SDN with reinforcement learning. 
- Except the environment can be defined along side with the controller in a single python script.
- Please refer to the [this example](https://github.com/amitnilams/sdwan-gym "this example") for implementation details.

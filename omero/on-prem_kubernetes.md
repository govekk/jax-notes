# New servers
ctome01ld thru ctome04ld
govekk has access and sudo, govekk_pa no access

## Linux version
NAME="Rocky Linux"
VERSION="9.2 (Blue Onyx)"
ID="rocky"
ID_LIKE="rhel centos fedora"
VERSION_ID="9.2"
PLATFORM_ID="platform:el9"
PRETTY_NAME="Rocky Linux 9.2 (Blue Onyx)"
ANSI_COLOR="0;32"
LOGO="fedora-logo-icon"
CPE_NAME="cpe:/o:rocky:rocky:9::baseos"
HOME_URL="https://rockylinux.org/"
BUG_REPORT_URL="https://bugs.rockylinux.org/"
SUPPORT_END="2032-05-31"
ROCKY_SUPPORT_PRODUCT="Rocky-Linux-9"
ROCKY_SUPPORT_PRODUCT_VERSION="9.2"
REDHAT_SUPPORT_PRODUCT="Rocky Linux"
REDHAT_SUPPORT_PRODUCT_VERSION="9.2"

## Ports currently open
`ss -tulpn`
```
Netid           State            Recv-Q           Send-Q                      Local Address:Port                        Peer Address:Port           Process           
udp             UNCONN           0                0                               127.0.0.1:323                              0.0.0.0:*                                
udp             UNCONN           0                0                                   [::1]:323                                 [::]:*                                
tcp             LISTEN           0                128                               0.0.0.0:22                               0.0.0.0:*                                
tcp             LISTEN           0                4096                            127.0.0.1:32772                            0.0.0.0:*                                
tcp             LISTEN           0                4096                            127.0.0.1:32770                            0.0.0.0:*                                
tcp             LISTEN           0                4096                            127.0.0.1:32771                            0.0.0.0:*                                
tcp             LISTEN           0                4096                            127.0.0.1:32768                            0.0.0.0:*                                
tcp             LISTEN           0                4096                            127.0.0.1:32769                            0.0.0.0:*                                
tcp             LISTEN           0                128                                  [::]:22                                  [::]:*
```

Only ports open are 22 (ssh) and 32768-32772 (Filenet)
Filenet miiight be added by Docker? The other new servers don't have it:
```
Netid          State           Recv-Q          Send-Q                   Local Address:Port                    Peer Address:Port          Process          
udp            UNCONN          0               0                            127.0.0.1:323                          0.0.0.0:*                              
udp            UNCONN          0               0                                [::1]:323                             [::]:*                              
tcp            LISTEN          0               128                            0.0.0.0:22                           0.0.0.0:*                              
tcp            LISTEN          0               128                               [::]:22                              [::]:*                              
```

When port-forwarding on ctomero01lt
```
tcp    LISTEN     0      128                                *:6080                                           *:*                   users:(("kubectl",pid=1273602,fd=8))
```

### Test ports
ctomero01lt to ctome01ld
```
(base) [govekk@ctomero01lt ~]$ nc -vz ctome01ld.jax.org 22
Ncat: Version 7.50 ( https://nmap.org/ncat )
Ncat: Connected to 10.6.64.70:22.
Ncat: 0 bytes sent, 0 bytes received in 0.02 seconds.
(base) [govekk@ctomero01lt ~]$ nc -vz ctome01ld.jax.org 6080
Ncat: Version 7.50 ( https://nmap.org/ncat )
Ncat: No route to host.
```

ctome01ld to ctomero01lt
```
[govekk@ctome01ld ~]$ nc -vz ctomero01lt.jax.org 22
Ncat: Version 7.91 ( https://nmap.org/ncat )
Ncat: Connected to 10.6.64.25:22.
Ncat: 0 bytes sent, 0 bytes received in 0.02 seconds.
[govekk@ctome01ld ~]$ nc -vz ctomero01lt.jax.org 5555
Ncat: Version 7.91 ( https://nmap.org/ncat )
Ncat: Connection refused.
[govekk@ctome01ld ~]$ nc -vz ctomero01lt.jax.org 6080
Ncat: Version 7.91 ( https://nmap.org/ncat )
Ncat: Connected to 10.6.64.25:6080.
Ncat: 0 bytes sent, 0 bytes received in 0.02 seconds.
```

ctome01ld or ctomero01lt to ctomerodev
```
(base) [govekk@ctomero01lt ~]$ nc -vz ctomerodev.jax.org 22
Ncat: Version 7.50 ( https://nmap.org/ncat )
Ncat: Connected to 10.6.64.145:22.
Ncat: 0 bytes sent, 0 bytes received in 0.02 seconds.
(base) [govekk@ctomero01lt ~]$ nc -vz ctomerodev.jax.org 5080
Ncat: Version 7.50 ( https://nmap.org/ncat )
Ncat: Connection refused.
```

"no route to host" vs "connection refused"?

from self
```
[govekk@ctome01ld ~]$ nc -vz ctome01ld.jax.org 5080
Ncat: Version 7.91 ( https://nmap.org/ncat )
Ncat: Connection refused.
```
but still no route from other
```
(base) [govekk@ctomero01lt ~]$ nc -vz ctome01ld.jax.org 5080
Ncat: Version 7.50 ( https://nmap.org/ncat )
Ncat: No route to host.
```

### Running omero.web with port-forward on 6080
```
[govekk@ctome01ld ~]$ nc -vz 127.0.0.1 6080
Ncat: Version 7.91 ( https://nmap.org/ncat )
Ncat: Connected to 127.0.0.1:6080.
Ncat: 0 bytes sent, 0 bytes received in 0.02 seconds.
[govekk@ctome01ld ~]$ nc -vz ctome01ld.jax.org 6080
Ncat: Version 7.91 ( https://nmap.org/ncat )
Ncat: Connected to 10.6.64.70:6080.
Ncat: 0 bytes sent, 0 bytes received in 0.02 seconds.
```

curl outputs nothing but doesn't error
```
[govekk@ctome01ld ~]$ curl ctome01ld.jax.org:6080
[govekk@ctome01ld ~]$ curl ctome01ld.jax.org:6081
curl: (7) Failed to connect to ctome01ld.jax.org port 6081: Connection refused
```

This line was also added to `ss -tulpn`
```
tcp     LISTEN   0        4096             0.0.0.0:6080           0.0.0.0:*      users:(("kubectl",pid=1205987,fd=8))
```

### Traceroute
traceroute from ctomero01lt
```
(base) [govekk@ctomero01lt ~]$ traceroute ctomero01lt.jax.org
traceroute to ctomero01lt.jax.org (10.6.64.25), 30 hops max, 60 byte packets
 1  ctomero01lt.jax.org (10.6.64.25)  0.054 ms  0.019 ms  0.023 ms
(base) [govekk@ctomero01lt ~]$ traceroute ctomerodev.jax.org
traceroute to ctomerodev.jax.org (10.6.64.145), 30 hops max, 60 byte packets
 1  ctomerodev.jax.org (10.6.64.145)  0.236 ms  0.187 ms  0.142 ms
(base) [govekk@ctomero01lt ~]$ traceroute ctome01ld.jax.org
traceroute to ctome01ld.jax.org (10.6.64.70), 30 hops max, 60 byte packets
 1  ctome01ld.jax.org (10.6.64.70)  0.185 ms !X  0.157 ms !X  0.087 ms !X
```
"!X means "communication administratively prohibited""

traceroute from local Mac
```
(base) MLI-JGM412:~ govekk$ traceroute ctomero01lt.jax.org
traceroute to ctomero01lt.jax.org (10.6.64.25), 64 hops max, 52 byte packets
 1  10.251.134.1 (10.251.134.1)  20.227 ms  17.818 ms  14.533 ms
 2  10.251.0.1 (10.251.0.1)  50.299 ms  20.259 ms  20.188 ms
 3  10.251.2.5 (10.251.2.5)  67.330 ms  33.706 ms  33.389 ms
 4  10.4.0.19 (10.4.0.19)  70.490 ms  33.291 ms  32.360 ms
 5  ctomero01lt.jax.org (10.6.64.25)  76.230 ms  31.187 ms  32.083 ms
(base) MLI-JGM412:~ govekk$ traceroute ctomerodev.jax.org
traceroute to ctomerodev.jax.org (10.6.64.145), 64 hops max, 52 byte packets
 1  10.251.134.1 (10.251.134.1)  20.264 ms  16.726 ms  17.579 ms
 2  10.251.0.1 (10.251.0.1)  22.467 ms  18.604 ms  20.860 ms
 3  10.251.2.5 (10.251.2.5)  37.441 ms  37.123 ms  32.755 ms
 4  10.4.0.19 (10.4.0.19)  98.129 ms  33.186 ms  33.885 ms
 5  ctomerodev.jax.org (10.6.64.145)  108.780 ms  28.764 ms  42.570 ms
(base) MLI-JGM412:~ govekk$ traceroute ctome01ld.jax.org
traceroute to ctome01ld.jax.org (10.6.64.70), 64 hops max, 52 byte packets
 1  10.251.134.1 (10.251.134.1)  22.846 ms  23.110 ms  20.756 ms
 2  10.251.0.1 (10.251.0.1)  20.105 ms  23.044 ms  19.822 ms
 3  10.251.2.5 (10.251.2.5)  34.772 ms  31.193 ms  31.771 ms
 4  10.4.0.19 (10.4.0.19)  36.281 ms  34.077 ms  31.938 ms
 5  ctome01ld.jax.org (10.6.64.70)  89.336 ms !X  29.860 ms !X  31.493 ms !X
```
"!X means "communication administratively prohibited""

traceroute using TCP
```
(base) [govekk@ctomero01lt ~]$ sudo traceroute -T -p 22 ctome01ld.jax.org
traceroute to ctome01ld.jax.org (10.6.64.70), 30 hops max, 60 byte packets
 1  ctome01ld.jax.org (10.6.64.70)  0.269 ms  0.206 ms  0.153 ms
(base) [govekk@ctomero01lt ~]$ sudo traceroute -T -p 5080 ctome01ld.jax.org
traceroute to ctome01ld.jax.org (10.6.64.70), 30 hops max, 60 byte packets
 1  ctome01ld.jax.org (10.6.64.70)  0.208 ms !X  0.155 ms !X  0.114 ms !X
```
vs dev
```
(base) [govekk@ctomero01lt ~]$ sudo traceroute -T -p 22 ctomerodev.jax.org
traceroute to ctomerodev.jax.org (10.6.64.145), 30 hops max, 60 byte packets
 1  ctomerodev.jax.org (10.6.64.145)  0.217 ms  0.181 ms  0.231 ms
(base) [govekk@ctomero01lt ~]$ sudo traceroute -T -p 5080 ctomerodev.jax.org
traceroute to ctomerodev.jax.org (10.6.64.145), 30 hops max, 60 byte packets
 1  ctomerodev.jax.org (10.6.64.145)  0.211 ms  0.204 ms  0.168 ms
```
"!X means "communication administratively prohibited""

```
(base) [govekk@ctomero01lt ~]$ ping -c 3 ctome01ld.jax.org
PING ctome01ld.jax.org (10.6.64.70) 56(84) bytes of data.
64 bytes from ctome01ld.jax.org (10.6.64.70): icmp_seq=1 ttl=64 time=0.244 ms
64 bytes from ctome01ld.jax.org (10.6.64.70): icmp_seq=2 ttl=64 time=0.227 ms
64 bytes from ctome01ld.jax.org (10.6.64.70): icmp_seq=3 ttl=64 time=0.272 ms

--- ctome01ld.jax.org ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2002ms
rtt min/avg/max/mdev = 0.227/0.247/0.272/0.025 ms
```

### Firewall rules
`sudo iptables -S`
```
-P INPUT ACCEPT
-P FORWARD DROP
-P OUTPUT ACCEPT
-N DOCKER
-N DOCKER-ISOLATION-STAGE-1
-N DOCKER-ISOLATION-STAGE-2
-N DOCKER-USER
-A FORWARD -j DOCKER-USER
-A FORWARD -j DOCKER-ISOLATION-STAGE-1
-A FORWARD -o br-7b7838ff9d15 -m conntrack --ctstate RELATED,ESTABLISHED -j ACCEPT
-A FORWARD -o br-7b7838ff9d15 -j DOCKER
-A FORWARD -i br-7b7838ff9d15 ! -o br-7b7838ff9d15 -j ACCEPT
-A FORWARD -i br-7b7838ff9d15 -o br-7b7838ff9d15 -j ACCEPT
-A FORWARD -o docker0 -m conntrack --ctstate RELATED,ESTABLISHED -j ACCEPT
-A FORWARD -o docker0 -j DOCKER
-A FORWARD -i docker0 ! -o docker0 -j ACCEPT
-A FORWARD -i docker0 -o docker0 -j ACCEPT
-A DOCKER -d 192.168.49.2/32 ! -i br-7b7838ff9d15 -o br-7b7838ff9d15 -p tcp -m tcp --dport 32443 -j ACCEPT
-A DOCKER -d 192.168.49.2/32 ! -i br-7b7838ff9d15 -o br-7b7838ff9d15 -p tcp -m tcp --dport 8443 -j ACCEPT
-A DOCKER -d 192.168.49.2/32 ! -i br-7b7838ff9d15 -o br-7b7838ff9d15 -p tcp -m tcp --dport 5000 -j ACCEPT
-A DOCKER -d 192.168.49.2/32 ! -i br-7b7838ff9d15 -o br-7b7838ff9d15 -p tcp -m tcp --dport 2376 -j ACCEPT
-A DOCKER -d 192.168.49.2/32 ! -i br-7b7838ff9d15 -o br-7b7838ff9d15 -p tcp -m tcp --dport 22 -j ACCEPT
-A DOCKER-ISOLATION-STAGE-1 -i br-7b7838ff9d15 ! -o br-7b7838ff9d15 -j DOCKER-ISOLATION-STAGE-2
-A DOCKER-ISOLATION-STAGE-1 -i docker0 ! -o docker0 -j DOCKER-ISOLATION-STAGE-2
-A DOCKER-ISOLATION-STAGE-1 -j RETURN
-A DOCKER-ISOLATION-STAGE-2 -o br-7b7838ff9d15 -j DROP
-A DOCKER-ISOLATION-STAGE-2 -o docker0 -j DROP
-A DOCKER-ISOLATION-STAGE-2 -j RETURN
-A DOCKER-USER -j RETURN
```

by comparison, on ctomero01lt.jax.org:
```
-P INPUT ACCEPT
-P FORWARD DROP
-P OUTPUT ACCEPT
-N DOCKER
-N DOCKER-ISOLATION-STAGE-1
-N DOCKER-ISOLATION-STAGE-2
-N DOCKER-USER
-A FORWARD -j DOCKER-USER
-A FORWARD -j DOCKER-ISOLATION-STAGE-1
-A FORWARD -o br-36140913368c -m conntrack --ctstate RELATED,ESTABLISHED -j ACCEPT
-A FORWARD -o br-36140913368c -j DOCKER
-A FORWARD -i br-36140913368c ! -o br-36140913368c -j ACCEPT
-A FORWARD -i br-36140913368c -o br-36140913368c -j ACCEPT
-A FORWARD -o docker0 -m conntrack --ctstate RELATED,ESTABLISHED -j ACCEPT
-A FORWARD -o docker0 -j DOCKER
-A FORWARD -i docker0 ! -o docker0 -j ACCEPT
-A FORWARD -i docker0 -o docker0 -j ACCEPT
-A DOCKER -d 192.168.49.2/32 ! -i br-36140913368c -o br-36140913368c -p tcp -m tcp --dport 32443 -j ACCEPT
-A DOCKER -d 192.168.49.2/32 ! -i br-36140913368c -o br-36140913368c -p tcp -m tcp --dport 8443 -j ACCEPT
-A DOCKER -d 192.168.49.2/32 ! -i br-36140913368c -o br-36140913368c -p tcp -m tcp --dport 5000 -j ACCEPT
-A DOCKER -d 192.168.49.2/32 ! -i br-36140913368c -o br-36140913368c -p tcp -m tcp --dport 2376 -j ACCEPT
-A DOCKER -d 192.168.49.2/32 ! -i br-36140913368c -o br-36140913368c -p tcp -m tcp --dport 22 -j ACCEPT
-A DOCKER-ISOLATION-STAGE-1 -i br-36140913368c ! -o br-36140913368c -j DOCKER-ISOLATION-STAGE-2
-A DOCKER-ISOLATION-STAGE-1 -i docker0 ! -o docker0 -j DOCKER-ISOLATION-STAGE-2
-A DOCKER-ISOLATION-STAGE-1 -j RETURN
-A DOCKER-ISOLATION-STAGE-2 -o br-36140913368c -j DROP
-A DOCKER-ISOLATION-STAGE-2 -o docker0 -j DROP
-A DOCKER-ISOLATION-STAGE-2 -j RETURN
-A DOCKER-USER -j RETURN
```

```
(base) MLI-JGM412:~ govekk$ ssh ctome02ld.jax.org
govekk@ctome02ld.jax.org's password: 
Last login: Mon Nov 13 15:26:59 2023 from 10.251.134.116
[govekk@ctome02ld ~]$ sudo iptables -S
-P INPUT ACCEPT
-P FORWARD ACCEPT
-P OUTPUT ACCEPT
```

### Netstat
```
(base) [govekk@ctomero01lt ~]$ netstat -lnp
(Not all processes could be identified, non-owned process info
 will not be shown, you would have to be root to see it all.)
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      -                   
tcp        0      0 127.0.0.1:25            0.0.0.0:*               LISTEN      -                   
tcp        0      0 0.0.0.0:6080            0.0.0.0:*               LISTEN      1273602/kubectl     
tcp        0      0 127.0.0.1:49163         0.0.0.0:*               LISTEN      -                   
tcp        0      0 127.0.0.1:49164         0.0.0.0:*               LISTEN      -                   
tcp        0      0 127.0.0.1:49165         0.0.0.0:*               LISTEN      -                   
tcp        0      0 127.0.0.1:49166         0.0.0.0:*               LISTEN      -                   
tcp        0      0 127.0.0.1:49167         0.0.0.0:*               LISTEN      -                   
tcp        0      0 0.0.0.0:111             0.0.0.0:*               LISTEN      -                   
tcp6       0      0 :::22                   :::*                    LISTEN      -                   
tcp6       0      0 ::1:25                  :::*                    LISTEN      -                   
tcp6       0      0 :::111                  :::*                    LISTEN      -                   
udp        0      0 0.0.0.0:111             0.0.0.0:*                           -                   
udp        0      0 192.168.49.1:123        0.0.0.0:*                           -                   
udp        0      0 172.17.0.1:123          0.0.0.0:*                           -                   
udp        0      0 10.6.64.25:123          0.0.0.0:*                           -                   
udp        0      0 127.0.0.1:123           0.0.0.0:*                           -                   
udp        0      0 0.0.0.0:123             0.0.0.0:*                           -                   
udp        0      0 0.0.0.0:1016            0.0.0.0:*                           -                   
udp6       0      0 :::111                  :::*                                -                   
udp6       0      0 fe80::42:17ff:fe40::123 :::*                                -                   
udp6       0      0 fe80::8c74:7fff:fe8:123 :::*                                -                   
udp6       0      0 fe80::42:82ff:fede::123 :::*                                -                   
udp6       0      0 fe80::250:56ff:fea4:123 :::*                                -                   
udp6       0      0 ::1:123                 :::*                                -                   
udp6       0      0 :::123                  :::*                                -                   
udp6       0      0 :::1016                 :::*                                -                   
Active UNIX domain sockets (only servers)
Proto RefCnt Flags       Type       State         I-Node   PID/Program name     Path
unix  2      [ ACC ]     STREAM     LISTENING     13084    -                    /run/gssproxy.sock
unix  2      [ ACC ]     STREAM     LISTENING     19348    -                    private/tlsmgr
unix  2      [ ACC ]     STREAM     LISTENING     19337    -                    public/pickup
unix  2      [ ACC ]     STREAM     LISTENING     13127    -                    /var/lib/sss/pipes/private/sbus-dp_jax.org.919
unix  2      [ ACC ]     STREAM     LISTENING     39514    -                    /var/run/docker.sock
unix  2      [ ACC ]     STREAM     LISTENING     16897    -                    /var/lib/sss/pipes/nss
unix  2      [ ACC ]     STREAM     LISTENING     19351    -                    private/rewrite
unix  2      [ ACC ]     STREAM     LISTENING     19341    -                    public/cleanup
unix  2      [ ACC ]     STREAM     LISTENING     19354    -                    private/bounce
unix  2      [ ACC ]     STREAM     LISTENING     19357    -                    private/defer
unix  2      [ ACC ]     STREAM     LISTENING     19344    -                    public/qmgr
unix  2      [ ACC ]     STREAM     LISTENING     19366    -                    public/flush
unix  2      [ ACC ]     STREAM     LISTENING     19360    -                    private/trace
unix  2      [ ACC ]     STREAM     LISTENING     19381    -                    public/showq
unix  2      [ ACC ]     STREAM     LISTENING     19363    -                    private/verify
unix  2      [ ACC ]     STREAM     LISTENING     19369    -                    private/proxymap
unix  2      [ ACC ]     STREAM     LISTENING     19372    -                    private/proxywrite
unix  2      [ ACC ]     STREAM     LISTENING     19375    -                    private/smtp
unix  2      [ ACC ]     STREAM     LISTENING     19378    -                    private/relay
unix  2      [ ACC ]     STREAM     LISTENING     239914603 -                    /var/run/screen/S-govekk/1273475.port-forward
unix  2      [ ACC ]     STREAM     LISTENING     19384    -                    private/error
unix  2      [ ACC ]     STREAM     LISTENING     19387    -                    private/retry
unix  2      [ ACC ]     STREAM     LISTENING     19390    -                    private/discard
unix  2      [ ACC ]     STREAM     LISTENING     19393    -                    private/local
unix  2      [ ACC ]     STREAM     LISTENING     19396    -                    private/virtual
unix  2      [ ACC ]     STREAM     LISTENING     38256    -                    /var/run/docker/libnetwork/a091b5da47e1.sock
unix  2      [ ACC ]     STREAM     LISTENING     19399    -                    private/lmtp
unix  2      [ ACC ]     STREAM     LISTENING     19402    -                    private/anvil
unix  2      [ ACC ]     STREAM     LISTENING     19405    -                    private/scache
unix  2      [ ACC ]     STREAM     LISTENING     17148    -                    /etc/opt/omi/conf/sockets/omi_tPRGMCWX5
unix  2      [ ACC ]     STREAM     LISTENING     20803    -                    /var/opt/omi/run/omiserver.sock
unix  2      [ ACC ]     STREAM     LISTENING     665      -                    /run/systemd/private
unix  2      [ ACC ]     STREAM     LISTENING     19067    -                    /var/lib/sss/pipes/pam
unix  2      [ ACC ]     STREAM     LISTENING     30381    -                    /run/containerd/containerd.sock.ttrpc
unix  2      [ ACC ]     STREAM     LISTENING     30385    -                    /run/containerd/containerd.sock
unix  2      [ ACC ]     STREAM     LISTENING     13083    -                    /var/lib/gssproxy/default.sock
unix  2      [ ACC ]     STREAM     LISTENING     33499    -                    /var/run/docker/metrics.sock
unix  2      [ ACC ]     STREAM     LISTENING     239586015 -                    /run/containerd/s/6d701b396c58cc0436fc38f67275de52ac826df2946206b45ca8bf049608369c
unix  2      [ ACC ]     STREAM     LISTENING     20550    -                    /var/lib/sss/pipes/private/sbus-monitor
unix  2      [ ACC ]     STREAM     LISTENING     19068    -                    /var/lib/sss/pipes/private/pam
unix  2      [ ACC ]     STREAM     LISTENING     2021     -                    /var/run/rpcbind.sock
unix  2      [ ACC ]     STREAM     LISTENING     1006     -                    /var/run/vmware/guestServicePipe
unix  2      [ ACC ]     STREAM     LISTENING     2029     -                    /run/dbus/system_bus_socket
unix  2      [ ACC ]     SEQPACKET  LISTENING     753      -                    /run/udev/control
unix  2      [ ACC ]     STREAM     LISTENING     755      -                    /run/lvm/lvmpolld.socket
unix  2      [ ACC ]     STREAM     LISTENING     758      -                    /run/lvm/lvmetad.socket
```

```
(base) [govekk@ctomero01lt ~]$ netstat -r
Kernel IP routing table
Destination     Gateway         Genmask         Flags   MSS Window  irtt Iface
default         gateway         0.0.0.0         UG        0 0          0 ens160
10.6.64.0       0.0.0.0         255.255.255.0   U         0 0          0 ens160
link-local      0.0.0.0         255.255.0.0     U         0 0          0 ens160
172.17.0.0      0.0.0.0         255.255.0.0     U         0 0          0 docker0
192.168.49.0    0.0.0.0         255.255.255.0   U         0 0          0 br-2e8e0717e37a
(base) [govekk@ctomero01lt ~]$ netstat -rn
Kernel IP routing table
Destination     Gateway         Genmask         Flags   MSS Window  irtt Iface
0.0.0.0         10.6.64.1       0.0.0.0         UG        0 0          0 ens160
10.6.64.0       0.0.0.0         255.255.255.0   U         0 0          0 ens160
169.254.0.0     0.0.0.0         255.255.0.0     U         0 0          0 ens160
172.17.0.0      0.0.0.0         255.255.0.0     U         0 0          0 docker0
192.168.49.0    0.0.0.0         255.255.255.0   U         0 0          0 br-2e8e0717e37a
```

```
[govekk@ctome01ld ~]$ netstat -lnp
(Not all processes could be identified, non-owned process info
 will not be shown, you would have to be root to see it all.)
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      -                   
tcp        0      0 127.0.0.1:32772         0.0.0.0:*               LISTEN      -                   
tcp        0      0 127.0.0.1:32770         0.0.0.0:*               LISTEN      -                   
tcp        0      0 127.0.0.1:32771         0.0.0.0:*               LISTEN      -                   
tcp        0      0 127.0.0.1:32768         0.0.0.0:*               LISTEN      -                   
tcp        0      0 127.0.0.1:32769         0.0.0.0:*               LISTEN      -                   
tcp6       0      0 :::22                   :::*                    LISTEN      -                   
udp        0      0 127.0.0.1:323           0.0.0.0:*                           -                   
udp6       0      0 ::1:323                 :::*                                -                   
raw6       0      0 :::58                   :::*                    7           -                   
raw6       0      0 :::58                   :::*                    7           -                   
Active UNIX domain sockets (only servers)
Proto RefCnt Flags       Type       State         I-Node   PID/Program name     Path
unix  2      [ ACC ]     STREAM     LISTENING     30777027 -                    /run/.heim_org.h5l.kcm-socket
unix  2      [ ACC ]     STREAM     LISTENING     890978   -                    /run/containerd/s/4d037a5f03fdb2b56c2b20d3cee222ffa6571306e49494a79978cebc3d65a1b2
unix  2      [ ACC ]     STREAM     LISTENING     33439185 1189018/systemd      /run/user/63604/systemd/private
unix  2      [ ACC ]     STREAM     LISTENING     30779297 -                    /run/systemd/journal/io.systemd.journal
unix  2      [ ACC ]     STREAM     LISTENING     33439194 1189018/systemd      /run/user/63604/bus
unix  2      [ ACC ]     STREAM     LISTENING     51329    -                    /run/user/0/systemd/private
unix  2      [ ACC ]     STREAM     LISTENING     51339    -                    /run/user/0/bus
unix  2      [ ACC ]     STREAM     LISTENING     30781113 -                    /var/lib/sss/pipes/pam
unix  2      [ ACC ]     STREAM     LISTENING     30797068 -                    /var/lib/sss/pipes/nss
unix  2      [ ACC ]     STREAM     LISTENING     30792338 -                    /var/lib/sss/pipes/pac
unix  2      [ ACC ]     STREAM     LISTENING     17442    -                    /run/systemd/userdb/io.systemd.DynamicUser
unix  2      [ ACC ]     STREAM     LISTENING     17443    -                    /run/systemd/io.system.ManagedOOM
unix  2      [ ACC ]     STREAM     LISTENING     17454    -                    /run/systemd/journal/stdout
unix  2      [ ACC ]     STREAM     LISTENING     30799113 -                    /var/lib/sss/pipes/private/sbus-monitor
unix  2      [ ACC ]     STREAM     LISTENING     30796273 -                    /var/lib/sss/pipes/private/sbus-dp_jax.org.98928
unix  2      [ ACC ]     STREAM     LISTENING     30781114 -                    /var/lib/sss/pipes/private/pam
unix  2      [ ACC ]     STREAM     LISTENING     869004   -                    /run/docker.sock
unix  2      [ ACC ]     STREAM     LISTENING     890022   -                    /run/containerd/containerd.sock.ttrpc
unix  2      [ ACC ]     STREAM     LISTENING     890024   -                    /run/containerd/containerd.sock
unix  2      [ ACC ]     STREAM     LISTENING     869024   -                    /var/run/docker/metrics.sock
unix  2      [ ACC ]     STREAM     LISTENING     858898   -                    /var/run/docker/libnetwork/96b7e9fdeb76.sock
unix  2      [ ACC ]     STREAM     LISTENING     50560    -                    /run/dbus/system_bus_socket
unix  2      [ ACC ]     STREAM     LISTENING     21868    -                    /run/irqbalance/irqbalance1440.sock
unix  2      [ ACC ]     STREAM     LISTENING     29825    -                    /run/lvm/lvmpolld.socket
unix  2      [ ACC ]     SEQPACKET  LISTENING     29827    -                    /run/systemd/coredump
unix  2      [ ACC ]     SEQPACKET  LISTENING     29829    -                    /run/udev/control
unix  2      [ ACC ]     STREAM     LISTENING     30748621 -                    /run/systemd/private
Active Bluetooth connections (only servers)
Proto  Destination       Source            State         PSM DCID   SCID      IMTU    OMTU Security
Proto  Destination       Source            State     Channel
```

```
[govekk@ctome01ld ~]$ netstat -r
Kernel IP routing table
Destination     Gateway         Genmask         Flags   MSS Window  irtt Iface
default         _gateway        0.0.0.0         UG        0 0          0 enp152s0f1np1
10.6.64.0       0.0.0.0         255.255.255.0   U         0 0          0 enp152s0f1np1
169.254.3.0     0.0.0.0         255.255.255.0   U         0 0          0 enp0s20f0u7u2c2
172.17.0.0      0.0.0.0         255.255.0.0     U         0 0          0 docker0
192.168.49.0    0.0.0.0         255.255.255.0   U         0 0          0 br-7b7838ff9d15
[govekk@ctome01ld ~]$ netstat -rn
Kernel IP routing table
Destination     Gateway         Genmask         Flags   MSS Window  irtt Iface
0.0.0.0         10.6.64.1       0.0.0.0         UG        0 0          0 enp152s0f1np1
10.6.64.0       0.0.0.0         255.255.255.0   U         0 0          0 enp152s0f1np1
169.254.3.0     0.0.0.0         255.255.255.0   U         0 0          0 enp0s20f0u7u2c2
172.17.0.0      0.0.0.0         255.255.0.0     U         0 0          0 docker0
192.168.49.0    0.0.0.0         255.255.255.0   U         0 0          0 br-7b7838ff9d15
```

## Troubleshooting with Phillip

`ifconfig`
ctome02ld: 10.6.64.71
ctomerodev: 10.6.64.145
ctomero01lt: 10.6.64.25

solution
```
sudo firewall-cmd --state # should not be running, if running we need to stop
sudo firewall-cmd --list-all
sudo systemctl stop firewalld
sudo systemctl disable firewalld # stops from starting when server reboots
sudo systemctl mask firewalld
```
Applied to all 4

## Side note: minikube certificate expired on ctomero01lt.jax.org?
```
[WARNING Service-Kubelet]: kubelet service is not enabled, please run 'systemctl enable kubelet.service'
W1113 19:59:39.065953    5495 certs.go:524] WARNING: could not validate bounds for certificate apiserver-kubelet-client: the certificate has expired: NotBefore: 2022-10-17 14:33:07 +0000 UTC, NotAfter: 2023-10-20 18:30:17 +0000 UTC
error execution phase certs/apiserver-kubelet-client: [certs] certificate apiserver-kubelet-client not signed by CA certificate ca: x509: certificate has expired or is not yet valid: current time 2023-11-13T19:59:39Z is after 2023-10-20T18:30:17Z
To see the stack trace of this error execute with --v=5 or higher
```
`minikube delete` then `minikube start`
```
(base) [govekk@ctomero01lt k8s_ymls]$ cat ~/.minikube/certs/ca.pem | openssl x509 -noout -enddate
notAfter=Oct  2 14:27:00 2025 GMT
```
New end date 2025

# What tool to use
https://www.padok.fr/en/blog/minikube-kubeadm-kind-k3s 
- minikube: best for easy local setup, but not good for production cluster, because only one node
    - certificate expired after a year and had to delete and start new cluster
        - https://github.com/kubernetes/minikube/issues/10122#issuecomment-758227950
- kubeadm
    - "kubeadm is a tool built to provide a simple way to bootstrap a minimum viable Kubernetes cluster that works for development and production environments. It can be used to set up a multi-node Kubernetes cluster" - https://kodekloud.com/blog/setup-kubernetes-cluster/
- ansible
    - can automate setup of kubernetes cluster?
- k3s
    - does not use docker as runtime, usually used for smaller machines
- kind
    - easier setup than kubeadm, but runs inside docker container so harder to access cluster network
- kubespray
    - deploy kubernetes cluster w/ ansible? 
    - https://github.com/kubernetes-sigs/kubespray 
    - uses kubeadm internally

# Plan
1. Assign one server (ctome01ld) for minikube, install minikube and docker, try running omero and accessing via browser
2. Assign one server for kubeadm, split into VMs and set up kubeadm cluster
3. Expand kubeadm cluster to another server
4. Run things from service accounts, not govekk

## Steps for minikube
### Install docker
https://www.digitalocean.com/community/tutorials/how-to-install-and-use-docker-on-rocky-linux-9
i.e. ok to install CentOS version of Docker because Rocky built on CentOS

### Install minikube
https://minikube.sigs.k8s.io/docs/start/
Automatically creating docker container CPU 2, Mem 64200 Mb - might be limiting minikube node to that?

###
Install kubectl
https://kubernetes.io/docs/tasks/tools/ 

## Steps for kubadm
- How to split into multiple VM nodes? Maybe just start with one master and one worker. Maybe just start on one of the two servers and figure out how to expand later
- Maybe disable swap?
- Need internet access? And set up bridge traffic across nodes??
- Install Docker as container runtime??
    - https://kubernetes.io/docs/setup/production-environment/container-runtimes/#docker
- Install kubeadm
- Figure out how to take snapshots of VMs

Someone setting up VM kubeadm for CKA exam:
https://rudimartinsen.com/2020/08/08/setting-up-a-kubernetes-cluster-vms/ 
- install vim, docker
- install kubelet kubeadm kubectl
- edit /etc/hosts with endpoint name
- kubeadm init `sudo kubeadm init --control-plane-endpoint kube-master:6443 --pod-network-cidr 192.168.150.0/23`
- copy paste the commands from kubeadm init to set up machine for running cluster
- set up shell completion for kubectl commands
- start calico: "curl https://docs.projectcalico.org/manifests/calico.yaml -O; vi calico.yaml; kubectl apply -f calico.yaml" - changes subnet value in yaml file
- kubeadm join 
    - first does some token create and looking for certificate hash
- enable workloads on controller: "kubectl taint nodes --all node-role.kubernetes.io/master-"

https://www.mirantis.com/blog/how-install-kubernetes-kubeadm/
- disable swap
- install kubelet kubeadm kubectl
- kubeadm init `kubeadm init --pod-network-cidr=192.168.0.0/16`
    - prints extra commands for setting up machine to run cluster
- start calico: `kubectl apply -f https://docs.projectcalico.org/v2.6/getting-started/kubernetes/installation/hosted/kubeadm/1.6/calico.yaml`
- allow workloads on controller: `kubectl taint nodes --all node-role.kubernetes.io/master-`

https://devopscube.com/setup-kubernetes-cluster-kubeadm/
- video: https://www.youtube.com/watch?v=xX52dc3u2HU 
- options for Vagrant VM
- enable iptables bridged traffic
- disable swap - kubeadm doesn't work well with swap? "From 1.28 kubeadm has beta support for using swap with kubeadm clusters"
- install container runtime (they use cri-o, we'll use docker)
- install kubeadm, kubectl, kubelet on all nodes
- kubeadm init on controller node:
```
IPADDR=$(curl ifconfig.me && echo "")
NODENAME=$(hostname -s)
POD_CIDR="192.168.0.0/16"
sudo kubeadm init --control-plane-endpoint=$IPADDR  --apiserver-cert-extra-sans=$IPADDR  --pod-network-cidr=$POD_CIDR --node-name $NODENAME --ignore-preflight-errors Swap
```
- start calico from controller node
- kubeadm join worker node
- set up kubernetes metrics server
- add kubeadm config to workstation so can kubectl from elsewhere

https://pwittrock.github.io/docs/setup/independent/create-cluster-kubeadm/#instructions
- install kubeadm on all nodes
- kubeadm init on controller node
- start calico: `kubectl apply -f http://docs.projectcalico.org/v2.3/getting-started/kubernetes/installation/hosted/kubeadm/1.6/calico.yaml`
- enable workloads on controller: `kubectl taint nodes --all node-role.kubernetes.io/master-`
- kubeadm join using token printed by init command

https://www.densify.com/kubernetes-tools/kubeadm/
- install docker on all nodes
- install kubeadm, kubectl, kubelet on all nodes
- `sudo kubeadm init --pod-network-cidr=10.244.0.0/16`
- kubeadm join worker node
- set up iptables bridge calls
- start CNI on controller node

last updated 2023?:
https://earthly.dev/blog/managing-k8s-with-kubeadm/
- iptables bridged traffic
- disable swap
- install docker
- configure cgroup driver to work with docker?
- install kubeadm, kubectl, kubelet
- `sudo kubeadm init --pod-network-cidr=192.168.0.0/16 --kubernetes-version=stable-1.13 --node-name master`
- start cni
- `kubeadm token create --print-join-command`
- kubeadm join

kubeadm on rocky
https://www.linuxtechi.com/install-kubernetes-on-rockylinux-almalinux/#Step_6_Install_Kubernetes_tools
https://www.howtoforge.com/how-to-setup-kubernetes-cluster-with-kubeadm-on-rocky-linux/
- set hostname
- disable swap
- adjust SELinux and firewall rules (might not need because stopped firewalld)
- add the overlay and br_netfilter kernel modules on all the nodes
- install container runtime
- `sudo yum install -y kubelet kubeadm kubectl --disableexcludes=kubernetes`
- `sudo kubeadm init --control-plane-endpoint=k8s-master01`
- copy commands output by init

# VMs
- VirtualBox
https://medium.com/@gunjangarge/create-kubernetes-cluster-using-kubeadm-on-ubuntu-virtualbox-step-by-step-68a3eeb1f74c
- QEMU - harder, cli
- Vagrant?

Maybe lets not - if I can run workloads on master then there's no reason we should need VMs

# kubeadm setup
1. disable swap on all nodes
```
sudo swapoff -a && sudo sed -i '/ swap / s/^\(.*\)$/#\1/g' /etc/fstab
```

2. install docker / containerd on all nodes
install on rocky: https://www.redswitches.com/blog/install-docker-on-rocky-linux/
```
sudo dnf config-manager --add-repo=https://download.docker.com/linux/centos/docker-ce.repo
sudo dnf update -y
sudo dnf install docker-ce docker-ce-cli containerd.io docker-compose-plugin
```

3. start containerd on all nodes
```
containerd config default | sudo tee /etc/containerd/config.toml >/dev/null 2>&1
sudo sed -i 's/SystemdCgroup \= false/SystemdCgroup \= true/g' /etc/containerd/config.toml
sudo systemctl restart containerd
sudo systemctl enable containerd
sudo systemctl status containerd
```

* might be missing SELinux permissive

4. install required kernel modules (br_netfliter has to do with bridged traffic) on all nodes
```
sudo modprobe overlay
sudo modprobe br_netfilter

cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
overlay
br_netfilter
EOF
```

5. systemctl params on all nodes
```
cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-iptables  = 1
net.bridge.bridge-nf-call-ip6tables = 1
net.ipv4.ip_forward                 = 1
EOF

sudo sysctl --system
```

6. install kubelet kubeadm kubectl on all nodes
```
cat <<EOF | sudo tee /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-\$basearch
enabled=1
gpgcheck=1
gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
exclude=kubelet kubeadm kubectl
EOF

dnf makecache

sudo yum install -y kubelet kubeadm kubectl --disableexcludes=kubernetes

sudo systemctl enable --now kubelet
```

7. kubeadm init on ctome02ld.jax.org
```
sudo kubeadm init --pod-network-cidr=192.168.0.0/16
```

```
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

8. start calico on ctome02ld.jax.org
https://docs.tigera.io/calico/latest/getting-started/kubernetes/quickstart
```
kubectl create -f https://raw.githubusercontent.com/projectcalico/calico/v3.27.0/manifests/tigera-operator.yaml
kubectl create -f https://raw.githubusercontent.com/projectcalico/calico/v3.27.0/manifests/custom-resources.yaml
kubectl get pods -n calico-system
```
Can also download resources manifest and edit for configuration
Default cidr in custom-resources manifest: cidr: 192.168.0.0/16

8. allow workloads on control plane
```
kubectl taint nodes --all node-role.kubernetes.io/control-plane-
kubectl get nodes # should say ready
```
Now called control-plane, not master

9. join ctome03ld.jax.org
```
sudo kubeadm join 10.6.64.71:6443 --token d0fj4d.9zushr6i7gz6c0kw \
	--discovery-token-ca-cert-hash sha256:cc7070b4c4c7e56438ea06a045968d8a5c6fd20ef8fdc3b743532e03012921c7
```

## kubeadm init output
```
[govekk@ctome02ld ~]$ sudo kubeadm init --pod-network-cidr=192.168.0.0/16
I1219 14:42:13.262607  471959 version.go:256] remote version is much newer: v1.29.0; falling back to: stable-1.28
[init] Using Kubernetes version: v1.28.4
[preflight] Running pre-flight checks
	[WARNING Hostname]: hostname "ctome02ld" could not be reached
	[WARNING Hostname]: hostname "ctome02ld": lookup ctome02ld on 10.7.0.254:53: no such host
[preflight] Pulling images required for setting up a Kubernetes cluster
[preflight] This might take a minute or two, depending on the speed of your internet connection
[preflight] You can also perform this action in beforehand using 'kubeadm config images pull'
W1219 14:42:20.033755  471959 checks.go:835] detected that the sandbox image "registry.k8s.io/pause:3.6" of the container runtime is inconsistent with that used by kubeadm. It is recommended that using "registry.k8s.io/pause:3.9" as the CRI sandbox image.
[certs] Using certificateDir folder "/etc/kubernetes/pki"
[certs] Generating "ca" certificate and key
[certs] Generating "apiserver" certificate and key
[certs] apiserver serving cert is signed for DNS names [ctome02ld kubernetes kubernetes.default kubernetes.default.svc kubernetes.default.svc.cluster.local] and IPs [10.96.0.1 10.6.64.71]
[certs] Generating "apiserver-kubelet-client" certificate and key
[certs] Generating "front-proxy-ca" certificate and key
[certs] Generating "front-proxy-client" certificate and key
[certs] Generating "etcd/ca" certificate and key
[certs] Generating "etcd/server" certificate and key
[certs] etcd/server serving cert is signed for DNS names [ctome02ld localhost] and IPs [10.6.64.71 127.0.0.1 ::1]
[certs] Generating "etcd/peer" certificate and key
[certs] etcd/peer serving cert is signed for DNS names [ctome02ld localhost] and IPs [10.6.64.71 127.0.0.1 ::1]
[certs] Generating "etcd/healthcheck-client" certificate and key
[certs] Generating "apiserver-etcd-client" certificate and key
[certs] Generating "sa" key and public key
[kubeconfig] Using kubeconfig folder "/etc/kubernetes"
[kubeconfig] Writing "admin.conf" kubeconfig file
[kubeconfig] Writing "kubelet.conf" kubeconfig file
[kubeconfig] Writing "controller-manager.conf" kubeconfig file
[kubeconfig] Writing "scheduler.conf" kubeconfig file
[etcd] Creating static Pod manifest for local etcd in "/etc/kubernetes/manifests"
[control-plane] Using manifest folder "/etc/kubernetes/manifests"
[control-plane] Creating static Pod manifest for "kube-apiserver"
[control-plane] Creating static Pod manifest for "kube-controller-manager"
[control-plane] Creating static Pod manifest for "kube-scheduler"
[kubelet-start] Writing kubelet environment file with flags to file "/var/lib/kubelet/kubeadm-flags.env"
[kubelet-start] Writing kubelet configuration to file "/var/lib/kubelet/config.yaml"
[kubelet-start] Starting the kubelet
[wait-control-plane] Waiting for the kubelet to boot up the control plane as static Pods from directory "/etc/kubernetes/manifests". This can take up to 4m0s
[apiclient] All control plane components are healthy after 4.501733 seconds
[upload-config] Storing the configuration used in ConfigMap "kubeadm-config" in the "kube-system" Namespace
[kubelet] Creating a ConfigMap "kubelet-config" in namespace kube-system with the configuration for the kubelets in the cluster
[upload-certs] Skipping phase. Please see --upload-certs
[mark-control-plane] Marking the node ctome02ld as control-plane by adding the labels: [node-role.kubernetes.io/control-plane node.kubernetes.io/exclude-from-external-load-balancers]
[mark-control-plane] Marking the node ctome02ld as control-plane by adding the taints [node-role.kubernetes.io/control-plane:NoSchedule]
[bootstrap-token] Using token: d0fj4d.9zushr6i7gz6c0kw
[bootstrap-token] Configuring bootstrap tokens, cluster-info ConfigMap, RBAC Roles
[bootstrap-token] Configured RBAC rules to allow Node Bootstrap tokens to get nodes
[bootstrap-token] Configured RBAC rules to allow Node Bootstrap tokens to post CSRs in order for nodes to get long term certificate credentials
[bootstrap-token] Configured RBAC rules to allow the csrapprover controller automatically approve CSRs from a Node Bootstrap Token
[bootstrap-token] Configured RBAC rules to allow certificate rotation for all node client certificates in the cluster
[bootstrap-token] Creating the "cluster-info" ConfigMap in the "kube-public" namespace
[kubelet-finalize] Updating "/etc/kubernetes/kubelet.conf" to point to a rotatable kubelet client certificate and key
[addons] Applied essential addon: CoreDNS
[addons] Applied essential addon: kube-proxy

Your Kubernetes control-plane has initialized successfully!

To start using your cluster, you need to run the following as a regular user:

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

Alternatively, if you are the root user, you can run:

  export KUBECONFIG=/etc/kubernetes/admin.conf

You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  https://kubernetes.io/docs/concepts/cluster-administration/addons/

Then you can join any number of worker nodes by running the following on each as root:

kubeadm join 10.6.64.71:6443 --token d0fj4d.9zushr6i7gz6c0kw \
	--discovery-token-ca-cert-hash sha256:cc7070b4c4c7e56438ea06a045968d8a5c6fd20ef8fdc3b743532e03012921c7 
```

## Testing kubeadm
```
kubectl create deployment nginx --image=nginx
kubectl get pod -o wide # says which node running on
kubectl expose deployment nginx --type=NodePort --port=80
kubectl get service
```
`curl clusterIP:80` only works on cluster nodes
`curl nodeIP:31087` where 31087 is the internal node port in 80:31087 works from anywhere (browser, ctome01ld.jax.org)
but only works on 30000-37xxx ports according to webpage

```
kubectl scale deployment/nginx --replicas=2
kubectl get pod -o wide # shows on both nodes
```

```
kubectl expose deployment nginx --type=ClusterIP \
--port=80 --target-port=80 --name=web
service_ip=$(kubectl get service web -o jsonpath='{.spec.clusterIP}')
curl $service_ip
```
curl works from either but cluster IP looks like 10.106.43.41 - because set by service - might need to set cluster endpoint
- browser doesn't work - timeout; ctome01lp.jax.org curl doesn't work - timeout

Cluster IP is by default only accessible from within cluster

Works if port-forward: 
```
kubectl port-forward svc/web 4080:80 --address=0.0.0.0
http://ctome02ld.jax.org:4080/
```

Need permanent load balancer

## Other packages required
for nfs-pv.yml (not permanent) - need nfs packages installed on nodes
```
sudo dnf install nfs-utils
```

# Networking
https://www.nginx.com/blog/kubernetes-networking-101/

MetalLB is most barebones external load balancer option?
https://itnext.io/kubernetes-loadbalancer-service-for-on-premises-6b7f75187be8 
https://metallb.universe.tf/

`sudo iptables -t nat -L KUBE-SERVICES`
`sudo iptables -t nat -L KUBE-SVC-R3GXWJKJPWWZNSJL`

## DNS entry created by Phillip
Name:    omero-dev.jax.org
Address:  10.6.64.126

## External IP
https://www.fadhil-blog.dev/blog/kubernetes-external-ip/ 

This works! But only for node IPs

Tried with omero-dev IP and other ctome01ld.jax.org IP:
```
curl 10.6.64.126
curl: (7) Failed to connect to 10.6.64.126 port 80: Connection refused

curl 10.6.64.70
curl: (7) Failed to connect to 10.6.64.70 port 80: Connection refused
```

## Calico can advertise cluster IPs externally
https://docs.tigera.io/calico/latest/networking/configuring/advertise-service-ips#big-picture 

Or can pass to MetalLB: https://metallb.universe.tf/configuration/calico/ 

# Container images
Should store images on private registry, not docker hub

## Upgrade OS
debian -> rocky?
upgrade python

## Change to glencoe ice wheels

# Database
Get DB login, though didn't end up working this way
```
ssh ctomerodev.jax.org
sudo su - svc-omerodev
source /local/omero_app/omerovenv/bin/activate
omero config get omero.db.user # omerodb
omero config get omero.db.pass
omero config get omero.db.name # omero_database
```

```
ssh ctomerodev.jax.org
sudo su - postgres
pg_dump omero_dev_database -F t > omero_dev_database_dump_011024.tar
```

```
kubectl apply -f postgres.yml # changed database user to omerodb
kubectl cp omero_database_dump_011024.tar database:/tmp
pg_restore -d omero omero_database_dump_011024.tar
```

* Need to use omerodb as OMERO database user
* Can change OMERO database name but probably shouldn't TODO: set database name and password to match


# Logging & monitoring

# Filesystem mounting
Permissions discussion: https://forum.image.sc/t/permission-settings-for-omero-server-in-virtual-machine/89757/3

Types of mounts:
- hostpath: mount a local directory, not allowed for multi-node
- local: mount a local disk/partition, works for multinode but must set node affinity on pod
- nfs

idr using nfs
public using nfs


Set up nfs mount on ctome01ld.jax.org: https://www.digitalocean.com/community/tutorials/how-to-set-up-an-nfs-mount-on-rocky-linux-9 at `nfs-test-share`
ctome01ld.jax.org: 10.6.64.70
ctome02ld.jax.org: 10.6.64.71
ctome03ld.jax.org: 10.6.64.72

### Temp NFS from ctomerodev.jax.org
```
sudo yum -y install nfs-utils
sudo systemctl start nfs-server
sudo /usr/sbin/exportfs -ar
```

/hyperfile
/local/OMERO

ctomerodev.jax.org = 10.6.64.145


Blitz.log
```
2024-01-11 21:33:00,012 WARN  [ o.services.blitz.fire.TopicManager$Impl] (2-thread-4) Could not cast to TopicManager
Ice.NoEndpointException: null
```

Error instantiating pixel buffer
Caused by: java.io.FileNotFoundException: /local/OMERO/ManagedRepository/user-1_205/2023-10/02/17-06-01.023/A1.pattern1.tif (No such file or directory)
Because I mounted at default /OMERO instead of /local/OMERO, let's try it again. Can't just change path, need to set OMERO.data.dir (CONFIG_omero_data_dir) to be /local/OMERO

## Where are caches?

## Where are log files?

## Lock files?

# Object storage mounting

# Backups
- backup etcd kubernetes database

# Run as different user

# LDAP and user authentication
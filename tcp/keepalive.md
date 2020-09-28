
# What
Keep TCP connection alive

# How
  * Timer
  * Probe packet with ACK flag and without data ==> why does this work?
  * Receive a reply with ACK flag and without data

# Why
  * Detect dead peers
  * Preventing disconnection due to network inactivity

### Detect dead peers
  * Situations
    1. Peer dead (machine power off) 
    2. Network gone (unplug cable)
    3. Network channel between peers gone (machine power off and then restore)

### Preventing disconnection due to network inactivity
  Heartbeat to prevent the connection depreciated by proxy or firewall

# In Practice

## Change Config
* procfs

```shell
  # cat /proc/sys/net/ipv4/tcp_keepalive_time
  7200
  # cat /proc/sys/net/ipv4/tcp_keepalive_intvl
  75
  # cat /proc/sys/net/ipv4/tcp_keepalive_probes
  9

  # echo 600 > /proc/sys/net/ipv4/tcp_keepalive_time
  # echo 60 > /proc/sys/net/ipv4/tcp_keepalive_intvl
  # echo 20 > /proc/sys/net/ipv4/tcp_keepalive_probes
```

* **sysctl**(8)

```shell
  # sysctl \
  > net.ipv4.tcp_keepalive_time \
  > net.ipv4.tcp_keepalive_intvl \
  > net.ipv4.tcp_keepalive_probes
  net.ipv4.tcp_keepalive_time = 7200
  net.ipv4.tcp_keepalive_intvl = 75
  net.ipv4.tcp_keepalive_probes = 9

  # sysctl -w \
  > net.ipv4.tcp_keepalive_time=600 \
  > net.ipv4.tcp_keepalive_intvl=60 \
  > net.ipv4.tcp_keepalive_probes=20
  net.ipv4.tcp_keepalive_time = 600
  net.ipv4.tcp_keepalive_intvl = 60
  net.ipv4.tcp_keepalive_probes = 20
```

## In Program

```C
  int setsockopt(int s, int level, int optname, const void *optval, socklen_t optlen)

  setsockopt(s, SOL_SOCKET, SO_KEEPALIVE, optval, optlen) 
  setsockopt(s, SOL_TCP, TCP_KEEPIDLE, optval, optlen) // overrides tcp_keepalive_time
  setsockopt(s, SOL_TCP, TCP_KEEPINTVL, optval, optlen) // overrides tcp_keepalive_time
  setsockopt(s, SOL_TCP, TCP_KEEPCNT, optval, optlen) // overrides tcp_keepalive_probes 
```


# Related Questions
  * What's the procedure if it turns out the connection is gone?

# Reference
[1][TCP Keepalive HOWTO](https://tldp.org/HOWTO/TCP-Keepalive-HOWTO/index.html)



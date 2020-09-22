
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

# Related Questions

# Reference




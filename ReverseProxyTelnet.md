# Brainstorming and Ideas for Setting up a Reverse Proxy
Could we still have DNS point at the home RPi, port 3349, but have _it_ then redirect requests to telnet running from work????

Check out [this thread](https://superuser.com/questions/1472153/what-are-my-best-options-if-desiring-to-tunnel-reverse-proxy-to-and-from-a-tel):

_(paraphrased)_ "IP Forwarding - a bbs/ssh runs here, but I want to run from a different (remote) machine while allowing connection requests to it at this machine:"
> There are lots of ways to do that. You can set up an SSH tunnel and connect your server via SOCKS proxy, or you can create a tunnel interface (OpenVPN, for instance) and bind the server to it.

> A more elegant solution might be to use NAT on your remote system:
``` bash
iptables -t nat -A PREROUTING -p tcp --dport 23 -j DNAT --to-destination <home_server_ip>:23
iptables -t nat -A POSTROUTING -j MASQUERADE
```
> You would also need to allow IP forwarding by adding net.ipv4.ip_forward=1 to /etc/sysctl.conf.

Ideally this could be done so that my RPi which serves the http server would forward telnet traffic out to my work server which reliably runs on a business cable network. I just don't quite know which IP in the above superuser thread answer actually serves the content (the telnet server), the "home IP" or the "remote system."

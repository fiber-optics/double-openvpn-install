## double-openvpn-install

> Client ---*encrypted*---> Server01 ---*encrypted*---> Server02 ---*https*---> Internet

### Installation
*On Server01:*
```
wget https://git.io/v51JE -O first-server-install.sh && bash first-server-install.sh
```

Client config is available at ~/client.ovpn
```
cd ~/ && python -m SimpleHTTPServer 7999 &
```

*And then on Client:*
```
wget http://Server01_IP:7999/client.ovpn -O /etc/openvpn/client.conf
```

And turn off simple web server on Server01! For ex.: 
```
kill -9 `pidof python`
```
--------------------------------------------------------------------------------

*On Server02:*
```
wget https://git.io/v51Ja -O second-server-install.sh && bash second-server-install.sh
```
watchout on interface iptables rules

Upstream config for Server01 is available at ~/upstream.conf:
```
cd ~/ && python -m SimpleHTTPServer 7999 &
```

*And then on Server01:*
```
wget http://Server02_IP:7999/upstream.conf -O /etc/openvpn/upstream.conf
wget http://Server02_IP::7999/upstream-route.sh -O /etc/openvpn/upstream-route.sh

```
And turn off simple web server on Server02

```
kill -9 `pidof python`
```

then delete SNAT-recod

```
iptables -t nat -D POSTROUTING 1
```

check iptables rules, it's looks like this:

```
iptables -L -nv -t nat
Chain PREROUTING (policy ACCEPT 0 packets, 0 bytes)
 pkts bytes target     prot opt in     out     source               destination

Chain INPUT (policy ACCEPT 0 packets, 0 bytes)
 pkts bytes target     prot opt in     out     source               destination

Chain OUTPUT (policy ACCEPT 0 packets, 0 bytes)
 pkts bytes target     prot opt in     out     source               destination

Chain POSTROUTING (policy ACCEPT 0 packets, 0 bytes)
 pkts bytes target     prot opt in     out     source               destination
    0     0 MASQUERADE  all  --  *      eth0    10.8.0.0/24         !10.8.0.0/24
    0     0 MASQUERADE  all  --  *      tap-upstream  0.0.0.0/0            0.0.0.0/0
```
and
```
nohup openvpn --config /etc/openvpn/upstream.conf &
```
--------------------------------------------------------------------------------

## Triple (Quad an so more) VPN
If you need Triple (and more) OpenVPN, then run on Server03:
```
wget https://git.io/v51Ja -O third-server-install.sh
sed -i 's/10.10/10.12/g' third-server-install.sh
sed -i 's/10.8/10.10/g' third-server-install.sh
bash third-server-install.sh
```
Upstream config for Server02 is available at ~/upstream.conf:
`cd ~/ && python -m SimpleHTTPServer 7999 &`

*And then on Server02:*
`wget http://Server03_IP:7999/client.ovpn -O /etc/openvpn/client.conf`

And turn off simple web server on Server03!

### Thanks
Based on https://github.com/Nyr/openvpn-install


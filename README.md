## double-openvpn-install

>>Client ---encrypted---> Server01 ---encrypted---> Server02 ---https---> Internet

### Installation
*On Server01:*
`wget https://git.io/vpn01 -O first-server-install.sh && bash first-server-install.sh`

Client config is available at ~/client.ovpn
`cd ~/ && python -m SimpleHTTPServer 7999 &`

*And then on Client:*
`wget http://Server01_IP:7999/client.ovpn -O /etc/openvpn/client.conf`

And turn off simple web server on Server01! For ex.:
```
kill -9 `pidof python`
```
--------------------------------------------------------------------------------

*On Server02:*
`wget https://git.io/vpn02 -O second-server-install.sh && bash second-server-install.sh`

Upstream config for Server01 is available at ~/upstream.conf:
`cd ~/ && python -m SimpleHTTPServer 7999 &`

*And then on Server01:*
`wget http://Server02_IP:7999/upstream.conf -O /etc/openvpn/upstream.conf`
And turn off simple web server on Server02!

--------------------------------------------------------------------------------

## Triple (Quad an so more) VPN
If you need Triple (and more) OpenVPN, then run on Server03:
```
wget https://git.io/vpn02 -O third-server-install.sh
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


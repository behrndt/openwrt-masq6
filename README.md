Masquerading NAT6
=================

Easy to use `firewall.nat6` script to allow you to specify `masq6` right as you'd expect.

Configuration is done per firewall zone, just like standard masquerading:

```
# in /etc/config/firewall:
config zone
        option name 'wan'
        option input 'DROP'
        option forward 'DROP'
        option output 'ACCEPT'
        option masq '1'
        option mtu_fix '1'
        option network 'wan wan6'

        ##
        ## Above is just an example, below are the nat6 related options:
        ##

        option masq6 '1'            # Enable masquerading NAT6
        # option masq6_privacy '1'  # Enable IPv6 privacy extensions
```

Installation
------------

Simply add the included `firewall.nat6` to `/etc/firewall.nat6`:

```sh
curl -sSLfo '/etc/firewall.nat6' 'https://raw.githubusercontent.com/jamesmacwhite/openwrt-masq6/master/firewall.nat6'
chmod +x '/etc/firewall.nat6'
```

Then add to your firewall config:

```sh
uci -q delete firewall.nat6
uci set firewall.nat6="include"
uci set firewall.nat6.path="/etc/firewall.nat6"
uci set firewall.nat6.reload="1"
uci commit firewall
```

Then configure as above and `/etc/init.d/firewall restart`.

You should also add this file to `/etc/sysupgrade.conf` to preserve this file on sysupgrade.

```
echo "/etc/firewall.nat6" >> /etc/sysupgrade.conf
```

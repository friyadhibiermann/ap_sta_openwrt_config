# ap_sta_openwrt_config
ap+sta wireless radio0 config
<pre>
# WIRELESS CONFIG
---------------------------------------------------
wireless.wifinet1=wifi-iface
wireless.wifinet1.ssid='CV.ONIVERSAL'
wireless.wifinet1.device='radio0'
wireless.wifinet1.mode='sta'
wireless.wifinet1.key='fryzilliazaqhira01'
wireless.wifinet1.network='wwan'
wireless.wifinet1.encryption='psk2'
wireless.wifinet1.bssid='34:A2:A2:1E:5F:8C'
wireless.wifinet2=wifi-iface
wireless.wifinet2.ssid='ONIVERSAL'
wireless.wifinet2.encryption='psk'
wireless.wifinet2.device='radio0'
wireless.wifinet2.mode='ap'
wireless.wifinet2.key='oniversal'
wireless.wifinet2.network='lan'
---------------------------------------------------
# CONFIG WIRELESS FILES
---------------------------------------------------
config wifi-iface 'wifinet1'
        option ssid 'CV.ONIVERSAL'
        option device 'radio0'
        option mode 'sta'
        option key 'fryzilliazaqhira01'
        option network 'wwan'
        option encryption 'psk2'
        option bssid '34:A2:A2:1E:5F:8C'

config wifi-iface 'wifinet2'
        option ssid 'ONIVERSAL'
        option encryption 'psk'
        option device 'radio0'
        option mode 'ap'
        option key 'oniversal'
        option network 'lan'
---------------------------------------------------
# CONFIG NETWORK
---------------------------------------------------
network.lan.netmask='255.255.255.0'
network.lan.ip6assign='60'
network.wwan=interface
network.wwan.proto='dhcp'
---------------------------------------------------
# CONFIG NETWORK FILES
---------------------------------------------------

config interface 'lan'
        option type 'bridge'
        option ifname 'eth0'
        option proto 'static'
        option ipaddr '192.168.1.1'
        option netmask '255.255.255.0'
        option ip6assign '60'

config interface 'wwan'
        option proto 'dhcp'
---------------------------------------------------
# CONFIG FIREWALL
---------------------------------------------------
firewall.@defaults[0]=defaults
firewall.@defaults[0].syn_flood='1'
firewall.@defaults[0].input='ACCEPT'
firewall.@defaults[0].output='ACCEPT'
firewall.@defaults[0].forward='ACCEPT'
firewall.@zone[0]=zone
firewall.@zone[0].name='lan'
firewall.@zone[0].input='ACCEPT'
firewall.@zone[0].output='ACCEPT'
firewall.@zone[0].forward='ACCEPT'
firewall.@zone[0].network='lan'
firewall.@include[0]=include
firewall.@include[0].path='/etc/firewall.user'
firewall.@zone[1]=zone
firewall.@zone[1].name='wwan'
firewall.@zone[1].mtu_fix='1'
firewall.@zone[1].input='ACCEPT'
firewall.@zone[1].forward='ACCEPT'
firewall.@zone[1].masq='1'
firewall.@zone[1].output='ACCEPT'
firewall.@forwarding[0]=forwarding
firewall.@forwarding[0].dest='wwan'
firewall.@forwarding[0].src='lan'
---------------------------------------------------
# CONFIG FIREWALL FILES
---------------------------------------------------
config defaults
        option syn_flood '1'
        option input 'ACCEPT'
        option output 'ACCEPT'
        option forward 'ACCEPT'

config zone
        option name 'lan'
        option input 'ACCEPT'
        option output 'ACCEPT'
        option forward 'ACCEPT'
        option network 'lan'

config include
        option path '/etc/firewall.user'

config zone
        option name 'wwan'
        option mtu_fix '1'
        option input 'ACCEPT'
        option forward 'ACCEPT'
        option masq '1'
        option output 'ACCEPT'

config forwarding
        option dest 'wwan'
        option src 'lan'
---------------------------------------------------
# CONFIG SYSTEM
---------------------------------------------------
system.@system[0]=system
system.@system[0].ttylogin='0'
system.@system[0].log_size='64'
system.@system[0].urandom_seed='0'
system.@system[0].zonename='UTC'
system.@system[0].hostname='ONIVERSAL'
system.@system[0].log_proto='udp'
system.@system[0].conloglevel='8'
system.@system[0].cronloglevel='5'
system.ntp=timeserver
system.ntp.server='0.openwrt.pool.ntp.org' '1.openwrt.pool.ntp.org' '2.openwrt.pool.ntp.org' '3.openwrt.pool.ntp.org'
system.led_lan=led
system.led_lan.name='LAN'
system.led_lan.sysfs='tp-link:green:lan'
system.led_lan.trigger='netdev'
system.led_lan.mode='link tx rx'
system.led_lan.dev='eth0'
---------------------------------------------------
# CONFIG SYSTEM FILES
---------------------------------------------------
config system
        option ttylogin '0'
        option log_size '64'
        option urandom_seed '0'
        option zonename 'UTC'
        option hostname 'ONIVERSAL'
        option log_proto 'udp'
        option conloglevel '8'
        option cronloglevel '5'

config timeserver 'ntp'
        list server '0.openwrt.pool.ntp.org'
        list server '1.openwrt.pool.ntp.org'
        list server '2.openwrt.pool.ntp.org'
        list server '3.openwrt.pool.ntp.org'

config led 'led_lan'
        option name 'LAN'
        option sysfs 'tp-link:green:lan'
        option trigger 'netdev'
        option mode 'link tx rx'
        option dev 'eth0'
---------------------------------------------------
# CONFIG DHCP FILES
---------------------------------------------------
config dnsmasq
        option domainneeded '1'
        option localise_queries '1'
        option rebind_protection '1'
        option rebind_localhost '1'
        option local '/lan/'
        option domain 'lan'
        option expandhosts '1'
        option authoritative '1'
        option readethers '1'
        option leasefile '/tmp/dhcp.leases'
        option localservice '1'
        list server '/oniversal.local/127.0.0.1'
        list server '/google.com/8.8.8.8'
        list server '/google.com/8.8.4.4'

config dhcp 'lan'
        option interface 'lan'
        option start '100'
        option limit '150'
        option leasetime '12h'
        option dhcpv6 'server'
        option ra 'server'

config odhcpd 'odhcpd'
        option maindhcp '0'
        option leasefile '/tmp/hosts/odhcpd'
        option leasetrigger '/usr/sbin/odhcpd-update'
        option loglevel '4'

config host
        option name 'oniversal.lan'
        option dns '1'
        option ip '192.168.1.1'
---------------------------------------------------
# CONFIG DHCP 
---------------------------------------------------
dhcp.@dnsmasq[0]=dnsmasq
dhcp.@dnsmasq[0].domainneeded='1'
dhcp.@dnsmasq[0].localise_queries='1'
dhcp.@dnsmasq[0].rebind_protection='1'
dhcp.@dnsmasq[0].rebind_localhost='1'
dhcp.@dnsmasq[0].local='/lan/'
dhcp.@dnsmasq[0].domain='lan'
dhcp.@dnsmasq[0].expandhosts='1'
dhcp.@dnsmasq[0].authoritative='1'
dhcp.@dnsmasq[0].readethers='1'
dhcp.@dnsmasq[0].leasefile='/tmp/dhcp.leases'
dhcp.@dnsmasq[0].localservice='1'
dhcp.@dnsmasq[0].server='/oniversal.local/127.0.0.1' '/google.com/8.8.8.8' '/google.com/8.8.4.4'
dhcp.lan=dhcp
dhcp.lan.interface='lan'
dhcp.lan.start='100'
dhcp.lan.limit='150'
dhcp.lan.leasetime='12h'
dhcp.lan.dhcpv6='server'
dhcp.lan.ra='server'
dhcp.odhcpd=odhcpd
dhcp.odhcpd.maindhcp='0'
dhcp.odhcpd.leasefile='/tmp/hosts/odhcpd'
dhcp.odhcpd.leasetrigger='/usr/sbin/odhcpd-update'
dhcp.odhcpd.loglevel='4'
dhcp.@host[0]=host
dhcp.@host[0].name='oniversal.lan'
dhcp.@host[0].dns='1'
dhcp.@host[0].ip='192.168.1.1'
</pre>

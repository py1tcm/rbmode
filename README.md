# rbMode

Tool for setting LAN/WAN mode on riverbed servers

## Modes

* n(oline): physically disconnect ethernet
* b(ridge): bridge interfaces (bypass mode)
* u(niversal): use interfaces as normal network interfaces

## Installation

~~~bash
aptitude install python2
wget https://raw.githubusercontent.com/c0d3z3r0/rbmode/master/rbmode.py
cp rbmode.py /usr/local/sbin/rbmode
chmod +x /usr/local/sbin/rbmode
echo i2c-dev >>/etc/modules

cat <<"EOF" >/etc/systemd/system/rbmode.service
[Unit]
Description=rbMode
Before=network-pre.target
Wants=network-pre.target

[Service]
Type=oneshot
ExecStart=/usr/local/sbin/rbmode <mode>  # Change <mode> to n, b or u

[Install]
WantedBy=network.target
EOF

systemctl enable rbmode.service
~~~
	
* Load i2c-dev and set the mode manually

	~~~bash
	modprobe i2c-dev
	rbmode u
	~~~

## Installation on OpenWrt 2X.XX

~~~bash
opkg install wget python3 python-smbus3 libi2c kmod-i2c-i801
wget https://raw.githubusercontent.com/py1tcm/rbmode/master/rbmode.py
cp rbmode.py /usr/bin/rbmode
chmod +x /usr/bin/rbmode
~~~

* Set the mode manually

	~~~bash
	rbmode u
	~~~
	
* Run at starup

	~~~bash
	vi /etc/rc.local
	rbmode u
	put <rbmode u> at top of <exit 0>
	~~~
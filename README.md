##Streamzap and MythTV
Getting the [Streamzap USB remote](http://www.streamzap.com/consumer/pc_remote/index.php) to work under Linux is currently trivial and does NOT require the use of [LIRC](http://www.lirc.org) any more, although LIRC does provide the ability to map the same keypress to different actions under a variety of applications.

The repo contains the file that works with ir-keytable and any modern Linux kernel, that will allow you to use your Streamzap USB remote with MythTV.

###Setting up:
* Install the ir-keytable package (your distro provides this in all likelyhood), but may not be a new enough version.  You will need ir-keytable 0.8.8 or higher.  To find out what version (if any) you currently have, from your terminal execute:
```
ir-keytable --version
```
You can find the package at https://launchpad.net/ubuntu/+source/v4l-utils/0.8.8-2ubuntu1 where you can get the source file to compile it yourself, but I recommend one of the built files that you can install by executing:
```
sudo dpkg -i <FILE>
```
* Modify /etc/rc_maps.cfg so the streamzap line points to /etc/rc_keymaps/streamzap.local
```
-*	rc-streamzap             streamzap
+*	rc-streamzap             /etc/rc_keymaps/streamzap.local
```

* Place streamzap.local to /etc/rc_keymaps.

* If you reboot, your remote should work at this point.  Alternately to rebooting, you can load the streamzap.local by executing the following command:
```
sudo ir-keytable -c -w /etc/rc_keymaps/streamzap.local
```


### Supplemental Info
#### Syntax of /etc/rc_keymaps/streamzap.local
The syntax of the keymap `scancode button_name`

#### Show Available Scancodes
Execute `ir-keytable` without any arguments.  Example:
```
# ir-keytable
Found /sys/class/rc/rc0/ (/dev/input/event3) with:
	Driver streamzap, table rc-streamzap
	Supported protocols: NEC RC-5 RC-6 JVC SONY SANYO LIRC RC-5-SZ other
	Enabled protocols: RC-5-SZ
	Name: Streamzap PC Remote Infrared Rec
	bus: 3, vendor/product: 0e9c:0000, version: 0x0100
	Repeat delay = 500 ms, repeat period = 125 ms
```

Execute `ir-keytable --read --device=/dev/input/PATH` where PATH is what the previous command outputted (event3 in the example above).

#### Show Availble Button Names
The lrc-utils package is likely required.  Execute `irrecord -l` to see a list of all available button names.


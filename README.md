cecd & piRemote
===============

cecd is an init script which can be used to run the cec-client utility from the
[libCEC][1] project as a daemon to provide an IP bridge for sending CEC commands
over an HDMI interface attached to the host. A telnet session to the configured
port will allow access to an interactive cec-client remotely. This script
requires [libCEC][1], including the cec-client utility, and [socat][2]. Default
runlevels and command-line options for cec-client are intended for running cecd
on a Raspberry Pi, but may be modified to run in any environment supported by
libCEC with an attached CEC-capable HDMI interface.

To manually run, you can use the command:
`socat tcp-listen:2600,reuseaddr,fork exec:"/usr/bin/cec-client $CEC_CLIENT_ARGS"`. However, this will kill cec-client after every connection is closed. As a hack-around, you can put the following commands into a script and run that to keep cec-client always running:
```bash
( ./socat -d -d PTY,raw,echo=0,link=/dev/ttyVA00 exec:"/usr/bin/cec-client $CEC_CLIENT_ARGS" ) &
sleep 5s
( ./socat -d -d open:/dev/ttyVA00,nonblock,echo=0,raw TCP-LISTEN:2600,reuseaddr,fork ) &
```

I'm looking into a better way around this, including writing a little daemon that instead of using cec-client and socat.

piRemote is a distribution of [Roomie Remote][3] device codes for use with the
Roomie Remote application and a Raspberri Pi running cecd and attached to the
HDMI interface of a TV supporting CEC. Adding the Raspberri Pi as a piRemote
device in Roomie Remote will allow IP control for basic functions (volume, power, digits, color codes, info etc). A full listing can be found in RoomieCodes.plist. Instructions for adding
custom device codes to Roomie Remote can be found [here][4].

Please note, every CEC implementation is different, so YMMV. I tested this with a Raspberry Pi and a Toshiba 55WX800U and all commands worked.

[1]: http://libcec.pulse-eight.com
[2]: http://freecode.com/projects/socat
[3]: http://www.roomieremote.com
[4]: http://www.roomieremote.com/support/#customDevice


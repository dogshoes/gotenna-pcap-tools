## Overview

Some utility programs for dealing with pcap files of goTenna BLE traffic produced by [Adafruit's Bluefruit LE Sniffer](https://www.adafruit.com/product/2269).

## Installation

Requires

1. Python 2.7+
2. pypcapfile
3. [Adafruit's Bluefruit LE Sniffer](https://www.adafruit.com/product/2269)
4. A pcap file produced by wireshark.  Pcapng is not supported.

Installing the prerequisite pypcapfile is easiest with `pip`.  You'll likely need your Linux distribution's version of libpcap, libpcap-devel, and python-devel.

```ShellSession
[jhe@oxcart gotenna-pcap-tools]$ pip install -r requirements.txt
```

## Use

There are currently two utility programs: `dump` and `extract-firmware`.

`dump` will produce a hexidecimal output of any Bluetooth Attribute Protocol frames that appear to be goTenna traffic.  It can be used for basic protocol analysis and discovery.  Feed this program a pcap file as STDIN and the results will be presented on STDOUT.

```ShellSession
[jhe@oxcart gotenna-pcap-tools]$ ./dump < gotenna-upgrade.pcap
little-endian capture file version 2.4
microsecond time resolution
snapshot length: 262144
linklayer type: None
number of packets: 19980


FRAME DIR OP HANDLE CONTENTS
------------------------------------------------------------
00096 M2S 12 001f   10020005000000000000000000c2701003
00098 S2M 1d 0021   1002400500000000000000000086a51003
00100 M2S 12 001f   1002000600000000000000000073bf1003
00102 S2M 1d 0021   10024006000000000000000000376a1003
00104 M2S 12 001f   100200070000000000000000001cfa1003
00106 S2M 1d 0021   10024007000000000000000000582f1003
00108 M2S 12 001f   100200080000000000000000004a4b1003
00110 S2M 1d 0021   100240080000000000000000000e9e1003
00112 M2S 12 001f   10020109003fff53edb53e5389ab571003
00114 S2M 1d 0021   1002c109b44c1003
```

`extract-firmware` will do its best to parse and output any packets containing firmware present in a pcap file.  Feed this program a pcap file on STDIN and it will output the binary firmware on STDOUT as well as any errors it encounters on STDERR.

```ShellSession
[jhe@oxcart gotenna-pcap-tools]$ ./extract-firmware < gotenna-upgrade.pcap > firmware.bin
Frame 155 is dupe: 0037c1010037c1010037c10100540a3a101008c0
Frame 181 failed CRC check.
Frame 213 is dupe: 1002090ed5404288422cd3260a3a10100a000001
Frame 381 failed CRC check.
Frame 517 failed CRC check.
...
```

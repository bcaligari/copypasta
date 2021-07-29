# tracing

* `strace` - trace system calls and signals
* `ltrace` - A library call tracer
* `tcpdump` - dump traffic on a network
* `wireshark` - Interactively dump and analyze network traffic

## strace

Intercepts and records system calls \(when an application requests a service
from the kernel\) and signals \(an asynchronous notification sent to a process\).

* `-s strsize` - increase the string size to print from the default of 32
* `-o filename` - save the trace to a file
* `-f` - attach to all threads
* `-t` - timestamp each line
* `-p pid` - attach to and trace a running process

Simplest is to call a program as a paramter to `strace`.

```{text}
strace ./someprog
```

System calls may have an entry section 2 of the man pages.

```{text}
man 2 open
```

It may be convenient to log the trace along with timestamps and child processes
traces to a file.


```{text}
strace -s 128 -f -t -o someprog.strace.txt ./someprog
```

It is also possible to connect strace to a running process.

```{text}
strace -s 128 -p 28875
```

## ltrace

Intercepts and records dynamic library calls.

* `-s strsize` - increase the string size to print from the default of 32
* `-o filename` - save the trace to a file
* `-f` - trace child processes
* `-t` - timestamp each line
* `-p pid` - attach to and trace a running process
* `-C` - demangle \(mostly to expose C++ function names\)

Library calls may have an entry section 3 of the man pages.

```{text}
man 3 puts
```

```{text}
ltrace -s 128 -f -t -o hello-c.ltrace.txt ./hello-c 
```

```{text}
ltrace -C -s 128 -f -t -o hello-cpp.ltrace.txt ./hello-cpp
```

## tcpdump

Record timestamps, headers, and payload information about matching packets on a network interface:

```{text}
       tcpdump [ -AbdDefhHIJKlLnNOpqStuUvxX# ] [ -B buffer_size ]
               [ -c count ]
               [ -C file_size ] [ -G rotate_seconds ] [ -F file ]
               [ -i interface ] [ -j tstamp_type ] [ -m module ] [ -M secret ]
               [ --number ] [ -Q in|out|inout ]
               [ -r file ] [ -V file ] [ -s snaplen ] [ -T type ] [ -w file ]
               [ -W filecount ]
               [ -E spi@ipaddr algo:secret,...  ]
               [ -y datalinktype ] [ -z postrotate-command ] [ -Z user ]
               [ --time-stamp-precision=tstamp_precision ]
               [ --immediate-mode ] [ --version ]
               [ expression ]
```

* `-i interface` - which interface
* `-n` - don't convert addresses such as host and port addresses, Ethernet OUIs, etc
* `-s snaplen` - truncate packets to `snaplan`
* `-e` - print link-level header such as Ethernet addres
* `-w` - write raw packets to a file
* `-c count` - exit after `count` packets
* `-r file` - read packets from a pcap file

Bind to an interface and list all traffic without name resolution.

```{text}
tcpdump -i eth1 -n
```

Show only UDP traffic to and from 172.16.1.61.

```{text}
tcpdump -i eth2 -n udp and host 172.16.1.61
```

Show the L2 address of ongoing traffic on eth2.

```{text}
tcpdump -i eth2 -n -e
```

Capture 20 ICMP datagrams to a file.

```{text}
tcpdump -i eth0 -c 20 -w icmp.pcap icmp
```

Display the captured traffic from a pcap file.

```{text}
tcpdump -n -r icmp.pcap
```

## Wireshark

Wireshark is a graphical tool to analyse packet data.  The data can be live
*captured* or loaded from a previously saved file such as a `tcpdump`
generated *pcap* file.

Traffic can be filtered on protocol and endpoints.

As Wireshark can be set up to understand a whole range of protocols streams can
be followed at various levels of encapsulation.

Configured with the appropriate keys, some encrypted traffic may also be
decoded.


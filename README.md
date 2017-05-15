# hepgen-bash

A HEP generator written in bash

## Features

`hep_send_log` sends HEP3 log packets to a capture node, using mostly bash (see **Requirements** for details).

Example:

```
$ hep_send_log -i 1337 10.0.0.42:9060 '123-abc-foo' 'This is a NaN% important log message'
```

sends a HEP3 log packet to **10.0.0.42:9060** with source capture id **1337**, **123-abc-foo** correlation ID and **This is a NaN% important log message** as a payload.

In combination with tools such as **tail** and **awk** or **grep**, it can be used to gather logs from any file and send them to Homer, without the requirement of any fancy dependency.

Example:

```bash
#!/bin/bash

LOGFILE=/path/to/logfile
HOMER_PEER=10.0.0.42:9060
CAPTURE_ID=1337
PATTERN='^ERROR'

tail -F "$LOGFILE" | grep --line-buffered "$PATTERN" |
    while read line
    do
        correlation_id=$(sed 's/^ERROR \([^ ]*\) .*$/\1/' <<< "$line")
        hep_send_log -i "$CAPTURE_ID" "$HOMER_PEER" "$correlation_id" "$line"
    done
```

While running this script, whenever a line such as

```
(...)
ERROR 46F5B0 problem with foo
(...)
```

appears in `/path/to/logfile`, the log line is sent to the homer peer, with **46F5B0** as correlation id.

## Usage

```
hep_send_log [OPTIONS] <DST IPV4>:<DST PORT> <CORRELATION ID> <MESSAGE>
OPTIONS:
  -i <ID>  : sets the HEP capture ID (default: 0)
  -k <KEY> : sets the HEP auth key (default: none)
  -s <IP4> : sets the HEP source IP (default: will use `ip route get` to guess)
  -h       : displays this message and exits
```

## Requirements

- **bash** (>=4)
- **xxd** (part of **vim**)
- **ip** (for source IP automatic discovery)
- GNU **date**

## License

LGPL-2.1

## Limitations

For now, `hep_send_log` only supports IPv4 as source or destination

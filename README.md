# hepgen-bash

A HEP generator written in bash

## Features

`hep_send_log` sends a HEP3 log packet to a capture node.

Example:

```
$ hep_send_log -i 1337 10.0.0.42:9060 '123-abc-foo' 'This is a NaN% important log message'
```

sends a HEP3 log packet to **10.0.0.42:9060** with source capture id **1337**, **123-abc-foo** correlation ID and **This is a NaN% important log message** as a payload.

## Requirements

- **bash** (>=4)
- **xxd** (part of **vim**)
- **ip** (for source IP automatic discovery)
- GNU **date**

## Limitations

For now, `hep_send_log` only supports IPv4 as source or destination

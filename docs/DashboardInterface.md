# Driver Station Dashboard Interface

The driver station can be interfaced by dashboards in 3 different methods; TCP, HTTP, or WebSocket. All methods use the same payload.

## Payload

Each message is a JSON string with the following payload. A new message is sent from the DS every time any of the fields updates, and on connection.

```
{
    "robotIP": "172.22.11.1", ; The Robot IP, in IP notation.
    "fmsControlled": true, ; Bool if the DS is FMS controlled or not.
    "dockedHeight": 200, ; Unsigned integer containing the DPI scaled height of the DS. 0 if not docked.
}
```

## Transport

### TCP

The TCP transport is running on port 6770 on the DS. Only connections from localhost will be accepted. The each payload message is delimited by a `\n` newline.

### WebSocket

The WebSocket can be accessed by connecting to `ws://localhost:6768/ipws` on the DS. Only connections from localhost will be accepted. Each message will come over as a `Text` message. The new line from the TCP message might be included, but can be discarded when using the WebSocket transport.

### HTTP

The HTTP GET function can be accessed at `http://localhost:6768/ip` on the DS. Only connections from localhost will be accepted. The GET response will the the JSON.

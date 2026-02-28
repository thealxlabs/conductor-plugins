# HomeKit Plugin for Conductor

Install: `conductor install homekit`

## Setup

**Authentication:** Homebridge URL

```bash
conductor plugins config homekit base_url http://homebridge.local:8581
`conductor plugins enable homekit`
```

Get credentials at: https://github.com/homebridge/homebridge

## Tools

```
homekit_status, homekit_rooms, homekit_accessories, homekit_get_accessory, homekit_set, homekit_toggle
```

Each tool is documented inline — ask Conductor what tools are available after installing.

## Source

Part of [thealxlabs/conductor-plugins](https://github.com/thealxlabs/conductor-plugins/tree/main/plugins/homekit).

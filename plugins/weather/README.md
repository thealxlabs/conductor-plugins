# Weather Plugin for Conductor

Install: `conductor install weather`

## Setup

**Authentication:** OpenWeatherMap API Key

```bash
conductor plugins config weather api_key \<KEY\>
`conductor plugins enable weather`
```

Get credentials at: https://openweathermap.org/api

## Tools

```
weather_current, weather_forecast, weather_hourly, weather_uv_index, weather_alerts
```

Each tool is documented inline — ask Conductor what tools are available after installing.

## Source

Part of [thealxlabs/conductor-plugins](https://github.com/thealxlabs/conductor-plugins/tree/main/plugins/weather).

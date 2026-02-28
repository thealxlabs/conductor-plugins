# Spotify Plugin for Conductor

Install: `conductor install spotify`

## Setup

**Authentication:** Client ID + Secret

```bash
conductor plugins config spotify client_id \<ID\> then client_secret \<SECRET\>
`conductor plugins enable spotify`
```

Get credentials at: https://developer.spotify.com/documentation/web-api

## Tools

```
spotify_now_playing, spotify_play, spotify_pause, spotify_next, spotify_previous, spotify_search, spotify_queue, spotify_playlists, spotify_create_playlist
```

Each tool is documented inline — ask Conductor what tools are available after installing.

## Source

Part of [thealxlabs/conductor-plugins](https://github.com/thealxlabs/conductor-plugins/tree/main/plugins/spotify).

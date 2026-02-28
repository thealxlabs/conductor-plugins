# X (Twitter) Plugin for Conductor

Install: `conductor install x`

## Setup

**Authentication:** Bearer Token + OAuth

```bash
conductor plugins config x bearer_token \<TOKEN\>
`conductor plugins enable x`
```

Get credentials at: https://developer.twitter.com/en/docs

## Tools

```
x_get_user, x_get_timeline, x_search, x_post_tweet, x_reply, x_like, x_retweet, x_delete_tweet
```

Each tool is documented inline — ask Conductor what tools are available after installing.

## Source

Part of [thealxlabs/conductor-plugins](https://github.com/thealxlabs/conductor-plugins/tree/main/plugins/x).

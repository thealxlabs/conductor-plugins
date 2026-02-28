# Vercel Plugin for Conductor

Install: `conductor install vercel`

## Setup

**Authentication:** API Token

```bash
conductor plugins config vercel token \<TOKEN\>
`conductor plugins enable vercel`
```

Get credentials at: https://vercel.com/docs/rest-api

## Tools

```
vercel_list_projects, vercel_list_deployments, vercel_get_deployment, vercel_get_logs, vercel_create_deployment, vercel_list_env, vercel_add_env
```

Each tool is documented inline — ask Conductor what tools are available after installing.

## Source

Part of [thealxlabs/conductor-plugins](https://github.com/thealxlabs/conductor-plugins/tree/main/plugins/vercel).

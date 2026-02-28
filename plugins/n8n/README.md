# n8n Plugin for Conductor

Install: `conductor install n8n`

## Setup

**Authentication:** API Key

```bash
conductor plugins config n8n api_key \<KEY\>
`conductor plugins enable n8n`
```

Get credentials at: https://docs.n8n.io/api

## Tools

```
n8n_list_workflows, n8n_get_workflow, n8n_trigger_webhook, n8n_list_executions, n8n_get_execution, n8n_activate_workflow, n8n_deactivate_workflow
```

Each tool is documented inline — ask Conductor what tools are available after installing.

## Source

Part of [thealxlabs/conductor-plugins](https://github.com/thealxlabs/conductor-plugins/tree/main/plugins/n8n).

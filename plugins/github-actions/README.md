# GitHub Actions Plugin for Conductor

Install: `conductor install github-actions`

## Setup

**Authentication:** Personal Access Token

```bash
conductor plugins config github token \<TOKEN\>
`conductor plugins enable github-actions`
```

Get credentials at: https://docs.github.com/en/rest/actions

## Tools

```
actions_list_workflows, actions_list_runs, actions_get_run, actions_trigger, actions_cancel, actions_rerun, actions_logs
```

Each tool is documented inline — ask Conductor what tools are available after installing.

## Source

Part of [thealxlabs/conductor-plugins](https://github.com/thealxlabs/conductor-plugins/tree/main/plugins/github-actions).

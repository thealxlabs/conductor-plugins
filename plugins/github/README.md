# GitHub Plugin for Conductor

Install: `conductor install github`

## Setup

**Authentication:** Personal Access Token

```bash
conductor plugins config github token \<TOKEN\>
`conductor plugins enable github`
```

Get credentials at: https://docs.github.com/en/rest

## Tools

```
github_get_user, github_get_repo, github_list_repos, github_search_code, github_list_issues, github_create_issue, github_list_prs, github_get_file
```

Each tool is documented inline — ask Conductor what tools are available after installing.

## Source

Part of [thealxlabs/conductor-plugins](https://github.com/thealxlabs/conductor-plugins/tree/main/plugins/github).

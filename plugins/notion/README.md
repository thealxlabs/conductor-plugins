# Notion Plugin for Conductor

Install: `conductor install notion`

## Setup

**Authentication:** Integration Token

```bash
conductor plugins config notion api_key \<TOKEN\>
`conductor plugins enable notion`
```

Get credentials at: https://developers.notion.com

## Tools

```
notion_search, notion_get_page, notion_create_page, notion_update_page, notion_query_database, notion_create_database_item
```

Each tool is documented inline — ask Conductor what tools are available after installing.

## Source

Part of [thealxlabs/conductor-plugins](https://github.com/thealxlabs/conductor-plugins/tree/main/plugins/notion).

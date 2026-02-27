# Plugin Authoring Guide

This guide covers everything you need to build a Conductor plugin and get it into the marketplace.

---

## Prerequisites

- Node.js 18+
- TypeScript 5+
- A working Conductor installation (`conductor status` should succeed)
- An account and API access for the service you're integrating

---

## 1. Scaffold Your Plugin

Create your plugin folder inside this repo:

```bash
mkdir -p plugins/my-service/src
cd plugins/my-service
```

Create `package.json`:

```json
{
  "name": "@conductor-plugins/my-service",
  "version": "1.0.0",
  "description": "One-line description",
  "type": "module",
  "main": "dist/my-service.js",
  "scripts": {
    "build": "tsc",
    "typecheck": "tsc --noEmit"
  },
  "devDependencies": {
    "typescript": "^5.0.0"
  }
}
```

Create `tsconfig.json`:

```json
{
  "compilerOptions": {
    "target": "ES2022",
    "module": "NodeNext",
    "moduleResolution": "NodeNext",
    "outDir": "./dist",
    "rootDir": "./src",
    "strict": true,
    "declaration": true,
    "skipLibCheck": true
  },
  "include": ["src/**/*"]
}
```

---

## 2. Implement the Plugin Interface

Create `src/index.ts`. Your plugin must export a class that matches the Conductor `Plugin` interface:

```typescript
import { Plugin, PluginTool } from '../../types.js'; // adjust path as needed
import { Conductor } from '../../core/conductor.js';
import { Keychain } from '../../security/keychain.js';

export class MyServicePlugin implements Plugin {
  name = 'my-service';         // must match your folder name and install ID
  description = 'One-line description shown in the marketplace';
  version = '1.0.0';

  private keychain!: Keychain;

  async initialize(conductor: Conductor): Promise<void> {
    this.keychain = new Keychain(conductor.getConfig().getConfigDir());
  }

  isConfigured(): boolean {
    // Return true if all required credentials exist
    // Or just return true and handle missing creds gracefully in handlers
    return true;
  }

  getTools(): PluginTool[] {
    return [
      // ... your tools here
    ];
  }
}
```

---

## 3. Define Tools

Each tool in `getTools()` has this shape:

```typescript
{
  name: 'my_service_do_thing',     // snake_case, namespaced with your plugin id
  description: 'What this tool does — written so an AI can understand when to use it',
  inputSchema: {
    type: 'object',
    properties: {
      param_name: {
        type: 'string',            // string | number | boolean | array | object
        description: 'What this parameter is for'
      },
      optional_param: {
        type: 'number',
        description: 'Optional number parameter'
      }
    },
    required: ['param_name']       // list required params only
  },
  requiresApproval: true,          // REQUIRED for any tool that writes, sends, or deletes
  handler: async ({ param_name, optional_param }: any) => {
    // implementation
    return { success: true, result: '...' };
  }
}
```

### Tool naming

Always namespace your tool names with your plugin ID:

```
✅ gmail_send
✅ gmail_search
✅ my_service_create_item

❌ send          (too generic)
❌ search        (conflicts with other plugins)
❌ createItem    (camelCase not allowed)
```

### Tool descriptions

Write descriptions that help the AI understand *when* to use the tool, not just *what* it does:

```
❌ "Send an email"
✅ "Send an email to one or more recipients. Use this when the user asks to send, forward, or reply to an email."

❌ "Get weather"  
✅ "Get current weather conditions for a location. Returns temperature, conditions, humidity, and wind speed."
```

### requiresApproval

Set `requiresApproval: true` on any tool that:
- Sends a message or email
- Creates, updates, or deletes a resource
- Triggers a workflow or deployment
- Uploads or modifies a file
- Posts to social media

Read-only tools (list, search, get) do **not** need approval.

---

## 4. Credential Handling

Use the Keychain API for all credentials. Never hardcode secrets or read them from environment variables.

```typescript
// In initialize():
this.keychain = new Keychain(conductor.getConfig().getConfigDir());

// Reading a credential:
const token = await this.keychain.get('my-service', 'api_key');
if (!token) {
  throw new Error(
    'my-service API key not configured.\n' +
    'Run: conductor plugins config my-service api_key <YOUR_KEY>'
  );
}

// The Keychain stores credentials encrypted at rest.
// Never log, return, or transmit credential values.
```

Keys are stored as `service / key` pairs. Convention:
- `service` = your plugin ID (e.g. `github`, `notion`, `my-service`)
- `key` = the credential name (e.g. `api_key`, `token`, `client_secret`)

---

## 5. Error Handling

Return useful errors. Don't let raw API errors bubble up:

```typescript
handler: async ({ query }: any) => {
  const token = await this.keychain.get('my-service', 'api_key');
  if (!token) {
    return { error: 'Not configured. Run: conductor plugins config my-service api_key <KEY>' };
  }

  try {
    const res = await fetch(`https://api.my-service.com/search?q=${encodeURIComponent(query)}`, {
      headers: { Authorization: `Bearer ${token}` }
    });

    if (res.status === 401) {
      return { error: 'API key is invalid or expired. Re-run: conductor plugins config my-service api_key <KEY>' };
    }

    if (!res.ok) {
      const text = await res.text().catch(() => res.statusText);
      return { error: `API error ${res.status}: ${text}` };
    }

    const data = await res.json();
    return { results: data.items, count: data.total };
  } catch (e: any) {
    return { error: `Network error: ${e.message}` };
  }
}
```

---

## 6. Write a README

Every plugin needs a `README.md`. Structure it like this:

```markdown
# My Service Plugin

Short description.

## Setup

1. Go to https://my-service.com/settings/api
2. Create an API key
3. Run: `conductor plugins config my-service api_key <YOUR_KEY>`
4. Run: `conductor plugins enable my-service`

## Tools

### my_service_list
Lists all items in your account.

**Parameters:** none

**Example output:**
\`\`\`json
{ "count": 3, "items": [{ "id": "abc", "name": "Item 1" }] }
\`\`\`

### my_service_create
Creates a new item. Requires approval.

**Parameters:**
- `name` (string, required) — Item name
- `priority` (string, optional) — "low", "medium", "high"

**Example output:**
\`\`\`json
{ "id": "xyz", "name": "My New Item", "priority": "high" }
\`\`\`

## Limitations

- API rate limit: 100 requests/minute
- Search requires at least 3 characters
```

---

## 7. Build and Test

```bash
cd plugins/my-service
npm install
npm run build      # compiles to dist/
npm run typecheck  # zero errors required
```

Test by symlinking into your local Conductor:

```bash
ln -s $(pwd)/dist/my-service.js ~/.conductor/plugins/my-service.js
conductor plugins enable my-service
conductor status
```

---

## 8. Submit

1. Commit your `src/`, `dist/`, `README.md`, `package.json`, and `tsconfig.json`
2. Open a PR against `main`
3. Open a [Plugin Submission](https://github.com/thealxlabs/conductor-plugins/issues/new?template=plugin-submission.yml) issue linking to your PR

---

## Common Mistakes

**Forgetting `requiresApproval`** — any tool that writes data needs it. When in doubt, add it.

**Returning raw API responses** — map the API response to a clean, consistent shape. The AI reads your return value directly.

**Not handling 401s** — expired tokens are common. Always check and return a helpful "re-authenticate" message.

**Tool names without namespace** — `send` will conflict with other plugins. Always prefix: `my_service_send`.

**Committing without building** — the `dist/` file is what Conductor actually downloads. Always `npm run build` before committing.

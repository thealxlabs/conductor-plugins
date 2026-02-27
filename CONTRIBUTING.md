# Contributing to conductor-plugins

Thanks for wanting to contribute. This is the official plugin registry for [Conductor](https://github.com/thealxlabs/conductor), and we want it to be high quality and trustworthy — since plugins run on users' machines with access to their credentials.

---

## Ways to Contribute

- **Submit a new plugin** — see [Authoring a Plugin](#authoring-a-plugin) below
- **Fix a bug in an existing plugin** — open a PR against the plugin's `src/` files
- **Improve docs** — fix typos, clarify setup steps, add examples
- **Report a bug** — open a [Bug Report](https://github.com/thealxlabs/conductor-plugins/issues/new?template=bug-report.yml)
- **Request a plugin** — open a [Plugin Request](https://github.com/thealxlabs/conductor-plugins/issues/new?template=plugin-request.yml)

---

## Authoring a Plugin

Read [docs/plugin-authoring.md](./docs/plugin-authoring.md) for the full guide. The short version:

**1. Create your plugin folder**

```
plugins/
└── my-service/
    ├── README.md
    ├── package.json
    ├── tsconfig.json
    ├── src/
    │   └── index.ts
    └── dist/
        └── my-service.js   ← compiled output
```

**2. Implement the Plugin interface**

```typescript
import { Plugin, PluginTool } from '@conductor/core';

export class MyServicePlugin implements Plugin {
  name = 'my-service';
  description = 'One-line description of what it does';
  version = '1.0.0';

  async initialize(conductor: Conductor): Promise<void> {
    // set up keychain, config, etc.
  }

  isConfigured(): boolean {
    return true; // or check if credentials exist
  }

  getTools(): PluginTool[] {
    return [
      {
        name: 'my_tool',
        description: 'What this tool does',
        inputSchema: {
          type: 'object',
          properties: {
            query: { type: 'string', description: 'The search query' }
          },
          required: ['query']
        },
        handler: async ({ query }) => {
          // implementation
          return { result: '...' };
        }
      }
    ];
  }
}
```

**3. Write a README for your plugin**

Every plugin needs a `README.md` covering:
- What it does
- Setup instructions (how to get credentials)
- Every tool with its parameters and an example
- Any known limitations

**4. Compile and test**

```bash
cd plugins/my-service
npm install
npx tsc
# verify dist/my-service.js was created
```

**5. Submit**

Open a [Plugin Submission](https://github.com/thealxlabs/conductor-plugins/issues/new?template=plugin-submission.yml) issue with a link to your PR.

---

## Code Standards

**Security**
- Never log or expose credentials — use the Keychain API only
- Never make network requests to your own servers — only the service the plugin integrates with
- Always validate inputs before using them in API calls
- Set `requiresApproval: true` on any tool that writes, deletes, or sends data

**TypeScript**
- Strict mode (`"strict": true` in tsconfig)
- No `any` unless genuinely unavoidable
- Explicit return types on public methods
- Handle errors gracefully — return useful error messages, don't throw unhandled

**Tools**
- Tool names must be namespaced: `gmail_send`, not just `send`
- Tool descriptions must be clear enough for an AI to use correctly without documentation
- `inputSchema` must be complete — every parameter described and typed
- Destructive operations must have `requiresApproval: true`

---

## Review Process

All PRs for new plugins go through:

1. **Automated checks** — TypeScript compiles, basic lint passes
2. **Security review** — code is read for credential handling, network calls, and data flows
3. **Functionality review** — tools do what they say, errors are handled, schema is complete
4. **Registry addition** — `registry.json` on the website is updated

We aim to review PRs within **5 business days**. Complex plugins may take longer.

---

## Code of Conduct

Be respectful. We're all here to build useful things. Issues and PRs that are abusive, spammy, or off-topic will be closed without comment.

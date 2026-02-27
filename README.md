# conductor-plugins

Official plugin registry for [Conductor](https://github.com/thealxlabs/conductor) — the AI integration hub by [TheAlxLabs](https://github.com/thealxlabs).

Each folder in `plugins/` is a self-contained plugin. Install any of them in seconds:

```bash
conductor install gmail
conductor install github
conductor install slack
```

Browse all available plugins at [conductor.thealxlabs.ca/marketplace](https://conductor.thealxlabs.ca/marketplace).

---

## Available Plugins

| Plugin | Category | Description | Install |
|--------|----------|-------------|---------|
| [gmail](./plugins/gmail) | Google | Read, send, search, and label emails | `conductor install gmail` |
| [gcal](./plugins/gcal) | Google | Create and manage calendar events | `conductor install gcal` |
| [gdrive](./plugins/gdrive) | Google | Search, read, and upload Drive files | `conductor install gdrive` |
| [github](./plugins/github) | Developer | Issues, PRs, repos, and code search | `conductor install github` |
| [github-actions](./plugins/github-actions) | Developer | Trigger and monitor CI workflows | `conductor install github-actions` |
| [notion](./plugins/notion) | Productivity | Pages, databases, and workspace search | `conductor install notion` |
| [slack](./plugins/slack) | Messaging | Send messages and search conversations | `conductor install slack` |
| [spotify](./plugins/spotify) | Media | Playback control and playlist management | `conductor install spotify` |
| [vercel](./plugins/vercel) | Developer | Deployments, logs, and project management | `conductor install vercel` |
| [n8n](./plugins/n8n) | Automation | Trigger and monitor n8n workflows | `conductor install n8n` |
| [weather](./plugins/weather) | Utilities | Current conditions and forecasts | `conductor install weather` |
| [x](./plugins/x) | Social | Post, search, and manage your X timeline | `conductor install x` |
| [homekit](./plugins/homekit) | Smart Home | Control HomeKit devices via Homebridge | `conductor install homekit` |

---

## How Plugins Work

When you run `conductor install <id>`, Conductor:

1. Fetches the registry at `conductor.thealxlabs.ca/registry.json`
2. Finds the plugin entry and its source path in this repo
3. Downloads the compiled `.js` file from this repo's `main` branch
4. Saves it to `~/.conductor/plugins/<id>.js`
5. Dynamically imports it — no restart required

Plugins are compiled TypeScript. The source lives in `plugins/<id>/src/`, the compiled output in `plugins/<id>/dist/`.

---

## Plugin Structure

Each plugin folder follows this layout:

```
plugins/
└── gmail/
    ├── README.md          # Plugin docs, setup instructions, tool reference
    ├── package.json       # Plugin metadata
    ├── tsconfig.json      # TypeScript config
    ├── src/
    │   └── index.ts       # Plugin source (implements Plugin interface)
    └── dist/
        └── gmail.js       # Compiled output — this is what Conductor downloads
```

The compiled `dist/<id>.js` must export a class that implements the Conductor `Plugin` interface:

```typescript
export class MyPlugin implements Plugin {
  name = 'my-plugin';
  description = 'What it does';
  version = '1.0.0';

  async initialize(conductor: Conductor): Promise<void> { /* setup */ }
  isConfigured(): boolean { /* return true if ready */ }
  getTools(): PluginTool[] { /* return tool definitions */ }
}
```

See [docs/plugin-authoring.md](./docs/plugin-authoring.md) for the full authoring guide.

---

## Contributing a Plugin

We welcome community plugins. Before submitting:

- Read [docs/plugin-authoring.md](./docs/plugin-authoring.md)
- Follow the [plugin structure](#plugin-structure) above
- Include a complete `README.md` in your plugin folder
- Make sure your plugin compiles cleanly with `tsc --noEmit`
- Open a [Plugin Submission](https://github.com/thealxlabs/conductor-plugins/issues/new?template=plugin-submission.yml) issue

All submitted plugins are reviewed for security and quality before being added to the registry.

---

## Reporting Issues

- **Bug in a plugin** → [Bug Report](https://github.com/thealxlabs/conductor-plugins/issues/new?template=bug-report.yml)
- **Plugin request** → [Plugin Request](https://github.com/thealxlabs/conductor-plugins/issues/new?template=plugin-request.yml)
- **Security vulnerability** → See [SECURITY.md](./SECURITY.md) — do **not** open a public issue

---

## Security

This repo distributes code that runs on your machine with access to your credentials and services. We take that seriously.

- All plugins are reviewed before being added to the registry
- Plugins run in the same process as Conductor — they are not sandboxed
- Credentials are stored in your local encrypted keychain, never sent to us
- If you find a vulnerability, report it privately via [SECURITY.md](./SECURITY.md)

---

## License

Apache 2.0 — see [LICENSE](./LICENSE).

Built by [TheAlxLabs](https://github.com/thealxlabs). Not affiliated with any of the services these plugins connect to.

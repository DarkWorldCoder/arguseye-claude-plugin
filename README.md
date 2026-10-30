# Arguseye for Claude Code

See a component, edit its source, and inspect the consequences without starting the application or writing stories. Arguseye renders actual React, browser-renderable Next.js and Vue 3 components with the project’s own packages. It returns screenshots, accessibility text, resolved inputs, warnings and errors.

This is a local review candidate. It is not listed in Anthropic’s directory and has not been submitted. The rendering engine, `@ayushniroula/arguseye@0.0.7`, is published on npm.

[Product](https://darkworldcoder.github.io/arguseye/) · [Technical walkthrough](https://darkworldcoder.github.io/arguseye/claude-code-ui.html) · [Engine source](https://github.com/DarkWorldCoder/arguseye)

## Requirements

- Claude Code with plugin support. The directory fields used here are recognized by version 2.1.281 and later.
- Node.js 22 or later, with npx on the executable path.
- A project with its dependencies installed and a Playwright-compatible Chromium browser or Google Chrome available.
- Vue projects need their own matching Vue runtime and compiler-sfc.

## Load locally for a session

Run Claude Code from the project you want to inspect, passing the absolute path of this plugin folder:

```sh
claude --plugin-dir /absolute/path/to/arguseye-plugin
```

The MCP configuration passes `${CLAUDE_PROJECT_DIR}` to the engine as an explicit `--root` argument. This keeps the source project separate from the plugin’s installation directory. The plugin’s MCP server uses stdio and is named arguseye; Claude adds a plugin namespace to the tool names.

This command does not add a permanent marketplace or install the plugin into your user settings. Do not enable a duplicate server from an earlier manual Arguseye setup in the same session.

## Inspect an edit

Ask Claude to find the usages of a component, render one with its discovered literal props, and report warnings and unresolved inputs. Save its capture ID. After your edit, compare against that baseline and review the before, after and highlighted pixel differences.

The bundled skill covers five tools: `render_component`, `render_diff`, `list_usages`, `render_usages` and `describe_props`. Usage renders are isolated call-site variants, with pagination and up to 12 renders per page. Each render identifies its target, inputs and viewport.

A comparison reports unchanged, changed or incomplete. A visual difference is evidence to review; it does not establish that a change is correct. Baselines retain the component export, resolved props and viewport. They do not replay actions, storage, parent frames or call-site context. Read the bundled skill’s replay limits before comparing stateful components.

## Execution and data handling

The plugin starts `npx -y @ayushniroula/arguseye@0.0.7 mcp` with the project root. npx can download that package and its dependencies from npm. The project’s own dependencies and source execute in a local preview runtime. The HTTP preview server binds to 127.0.0.1. The engine caches dependency bundles under the project’s node_modules/.arguseye/deps.

MCP results contain rendered content, accessibility text, paths, resolved props, warnings and errors. The coding agent receives those results, so the agent provider’s data handling applies. Component code can make its own network requests. Use the plugin with projects you trust and avoid previewing private data you do not want to send to your agent. This bundle adds no analytics hooks or remote MCP endpoint.

MCP baselines are kept in session memory, with the most recent 64 captures retained. CLI captures persist only when you explicitly choose an output or capture directory. Restarting MCP or evicting a capture invalidates its ID. No account or API key is required by Arguseye itself.

## Platform and framework limits

The local MCP workflow is intended for Claude Code with access to your project files and packages. The MCP server is not a remote connector for claude.ai chat. Cowork’s local plugin support differs; it has not been validated for this candidate. No cloud rendering or universal cross-app support is claimed.

Runtime-only props, missing providers, unsettled assets, unsupported Next.js server behavior and Nuxt context remain incomplete. Use application-level browser tests for authenticated, backend-dependent and complete application journeys.

The VS Code visual panel is a separate installation from the [Marketplace](https://marketplace.visualstudio.com/items?itemName=ayushniroula.arguseye).

## Support and license

[Report a problem](https://github.com/DarkWorldCoder/arguseye/issues), including the returned errors and warnings with private values removed. The plugin and engine are MIT-licensed. Created by Ayush Niroula.

# Privacy and data handling for the Arguseye Claude Code plugin

Last updated: 30 October 2026. This policy is for the Arguseye Claude Code plugin maintained by Ayush Niroula. It describes the local plugin configuration and the published `@ayushniroula/arguseye@0.0.7` engine that it starts. It does not describe the data practices of Anthropic, npm, GitHub, or the application code you choose to render.

## What the plugin reads

To find and render a component, Arguseye reads files, imports, component usage information, styles, assets, configuration and installed packages in the project you select. Render inputs can include props, component state, browser storage and application context. These inputs and rendered content can contain personal or confidential information if your project includes it. The tool does not automatically remove such information.

## Local execution and storage

The plugin starts a local MCP process through `npx` and renders project code in Chromium. Its preview HTTP server binds to `127.0.0.1`. The project’s own code and packages execute as part of rendering.

The engine stores dependency bundles under the project’s `node_modules/.arguseye/deps` directory. npm may also retain the downloaded engine and dependencies in its own cache. These files can remain after a preview session ends; users control their local files and caches.

The engine keeps render results and preview state in the running process and browser session. The baseline cache retains the latest 64 captures; an evicted or restarted-session capture ID no longer identifies a usable baseline. This baseline limit is not a claim that all process memory or local files are deleted after 64 renders.

If you separately invoke the CLI with an output or capture directory, it writes images and JSON evidence there. These files can contain screenshots, accessibility text, props, paths and errors. They remain until you remove them. The plugin configuration does not automatically export CLI captures.

## What leaves the preview process

MCP responses send requested screenshots, accessibility text, file paths, resolved inputs, warnings, errors and comparison evidence back to the coding agent. The agent may send that material to its provider under the provider’s own policies. Choosing text-only output omits the image but does not remove sensitive values from text evidence.

The plugin bundle declares one local MCP server and adds no analytics hooks or remote MCP endpoint. It does not provide an Arguseye-operated cloud collection service. Installation can contact npm and its package infrastructure to fetch the pinned engine and dependencies. The application code being previewed can make its own network requests, load external assets, or use services configured by your project; those requests are outside this bundle’s data-handling controls.

## Your choices

Use trusted projects and dependencies. Review source, inputs, warnings and outputs before allowing an agent to share them. Avoid previewing personal, confidential or production data you do not want your coding-agent provider to receive. You can stop the MCP process, close its browser session, remove local captures and caches, and disable the plugin in your client. Removing local data does not delete material already retained by a coding-agent provider, a package service, or your application’s services.

## Contact

Questions about this plugin can be raised through [Arguseye issues](https://github.com/DarkWorldCoder/arguseye/issues). Issues are public: do not include private source, credentials, personal data or unredacted error output. The project does not require an Arguseye account or API key. This policy does not make a retention promise on behalf of third-party services.

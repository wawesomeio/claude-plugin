# wawesome for Claude Code

Deploy JavaScript Functions and static pages to [wawesome](https://wawesome.io) without leaving
Claude Code.

An App holds Functions. A Function is one JavaScript module whose default export has a `fetch`
handler, and the static files a deploy carries are served beside it. There is no build step and no
bundler, so a deploy serves exactly what it declared.

## Install

```
/plugin install wawesome
```

The first tool call opens a browser tab on `auth.wawesome.io`. Sign in with GitHub or Google, and
pick what the connection may do. If you have no wawesome workspace yet, that screen creates one.
Installing the plugin is the whole of signing up.

There is no client ID and no client secret to enter anywhere, and you are never asked to paste a
token into a chat.

## What you get

**The MCP endpoint**, `https://api.wawesome.io/v1/mcp`. It lists your Apps and Functions, answers
the source a Version is running, deploys code and files together, invokes a handler, fetches a page
back off the live site, reads invocation logs, diagnoses the latest failure, rolls a Version back,
sets environment variables and attaches custom domains.

**Three skills**, which Claude reads when the work calls for them.

| Skill | What it is for |
|:---|:---|
| `wawesome-setup` | The consent screen's three jobs, which tools each one carries, and what to do when a tool refuses |
| `wawesome-platform` | What a deploy can carry from a chat, how to check what is live, and the work that belongs to the dashboard |
| `wawesome-cli` | Deploying a project that runs a framework build |

**Two commands.** `/wawesome-deploy` deploys this project and fetches the result back.
`/wawesome-diagnose` works out why a Function is failing.

## The endpoint and the CLI

The endpoint does nearly everything, and it does it better: one round trip, structured answers, no
shell.

The CLI covers the one thing the endpoint cannot. A project that runs a framework build has to be
built somewhere, and the platform builds nothing. So `npx wawesome deploy` runs the build on your
machine and uploads what it wrote. That is the only trigger. Files on your own disk are not one:
ask for them in the conversation and the agent calls `request_upload`, which answers a link that
opens for you and nobody else.

The CLI needs Node 22.13 or newer and nothing else.

## Permissions

`.mcp.json` cannot ask for a scope, so you pick a job on the consent screen yourself:

- **Read and investigate** writes nothing.
- **Ship and read back** adds deploying, rolling back, invoking, fetching and setting environment
  variables. This is the one to pick for building.
- **Build in the workspace** adds pausing schedules and creating previews, which no tool on this
  endpoint asks for.

None of the three attaches a custom domain. That is done on the dashboard or from the CLI.

The connection is one credential in your workspace, listed at Workspace Settings, then Credentials.
Revoke it there and its very next call is refused.

## Development

```bash
claude plugin validate --strict .
```

Every pull request runs that. `main` is protected, because a push to this repository reaches the
plugin directory with nothing in between.

## Links

- [MCP documentation](https://wawesome.io/docs/mcp)
- [CLI reference](https://wawesome.io/docs/cli)
- [Getting started](https://wawesome.io/docs/getting-started)

## License

MIT. See [LICENSE](./LICENSE).

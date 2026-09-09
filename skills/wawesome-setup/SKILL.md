---
name: wawesome-setup
description: Use when connecting to wawesome for the first time, when a wawesome tool refuses with "credential-missing-capability", or when someone asks how to sign in to wawesome or how to install its CLI. Names the three jobs on the consent screen, the tools each one carries, and what none of them carries.
---

# Connecting to wawesome

There are two credentials here and they are separate. The MCP endpoint holds one, the CLI holds
another. Signing in to one does not sign in to the other.

## The MCP endpoint

The first wawesome tool call opens a browser tab on `auth.wawesome.io`. The person signs in with
GitHub or Google. If they have no workspace yet, that same screen creates one. Installing this
plugin is the whole of signing up.

Then a consent screen asks what the connection may do. `.mcp.json` has no field for this, so the
plugin cannot pick for them. They pick, and what they pick decides which tools work for the next
year.

Tell them which job carries the tools they want.

### Read and investigate

Writes nothing. It carries `whoami`, `list_apps`, `list_functions`, `list_versions`,
`get_function_source`, `list_uploads`, `deploy_status`, `list_invocations`, `get_invocation`,
`read_invocation_logs`, `diagnose_latest_failure`, `get_usage` and `check_domain`.

This is the job for reading a live site, following a failure, or answering a question about the
workspace.

### Ship and read back

Everything above, and `deploy_function`, `rollback_function`, `request_upload`, `set_env_var`,
`invoke_function` and `fetch_function`.

This is the job for building. It is the whole loop: write the code, deploy it, fetch the page back,
read the logs, roll back if it went wrong.

### Build in the workspace

Everything in "Ship and read back", and pausing schedules and creating previews on top.

No tool on this endpoint asks for either of those two. Against this plugin it reaches exactly what
"Ship and read back" reaches. Worth picking if the same workspace hands the grant to something else
as well. Otherwise pick the narrower one.

### What no job carries

None of the three attaches a custom domain. `attach_domain` and `cancel_domain_claim` ask for
`write:domains`, and no job on that screen grants it. A domain is attached from the dashboard, or
from the CLI, which acts as the person themselves rather than as this connection.

`list_templates` and `get_template` ask for nothing at all, so the template catalogue is readable
whichever job they pick.

### Below the job

The screen also asks which Apps the connection reaches. Left alone it reaches every App, including
the ones created later.

A person can only grant what their own role in the workspace carries. A job carrying a word they do
not hold cannot be picked.

## When a tool refuses

A refusal that reads `credential-missing-capability` names every word it is short of at once. That
is an answer, not a hiccup. The same call with the same connection will refuse again.

Report the words, and say that the way past it is to connect again and approve a job that carries
them. Do not retry, and do not go looking for another tool that does the same thing.

## Changing or revoking it later

The connection is one row in the workspace, listed for the owner at Workspace Settings, then
Credentials, marked `Connector`. Its token refreshes itself in place. Revoking it there stops the
very next call.

## The CLI

The CLI is a separate credential on a separate flow. It is only needed for a framework build. See
the `wawesome-cli` skill for when that is.

Node 22.13 or newer is the whole requirement. There is nothing to install: `npx` fetches the CLI
each time it is named.

```bash
npx wawesome login
```

That opens a browser, signs the person in through GitHub, and writes
`~/.wawesome/credentials.json`. The token carries their own role in the workspace, which is usually
wider than any job on the consent screen. `npx wawesome whoami` prints who it belongs to and which
gateway it works at.

A token is only valid at the gateway that issued it. For a local or self-hosted one, pass
`npx wawesome login --gateway http://localhost:3000`.

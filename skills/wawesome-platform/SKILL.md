---
name: wawesome-platform
description: Use when working on a wawesome App, Function or Version. Covers what a deploy from this client can and cannot carry, how to check what is live rather than what is on disk, and the work that belongs to the dashboard or the CLI instead of a tool call.
---

# Working on wawesome from Claude Code

An App holds Functions. A deploy of a Function makes a Version, and a Version is the code and the
static files that one deploy declared together. A Function is one JavaScript module whose default
export has a `fetch` handler. Pages are files carried beside that handler, served straight out of
storage, and no code runs to answer one. There is no build and no bundler here, so a deploy serves
exactly what it declared.

The endpoint sends the rest of the model with its own instructions, including where each file
answers. What follows is what a Claude Code session needs on top of that.

## The working directory is not what is live

Two different things can be called the code. The files in this directory are one. The Version the
Function is running is the other, and they drift apart the moment somebody deploys from elsewhere.

Read before you write. `get_function_source` answers what the live Version runs, so a change is an
edit to that rather than a rewrite of a page somebody already published.

Nothing syncs on its own. A file in this directory reaches the platform only when a deploy declares
it.

## What a deploy from here can carry

A file written or read in this session travels in the deploy itself, as the `contents` of its
`assets` entry. Text only, at most 512 KB for one file and 2 MB across the deploy.

Anything binary, and anything larger, is declared by `content_hash` and its bytes uploaded
separately. This client has no way to make that upload. So a logo, a photograph, a font or a video
is asked for with `request_upload`, which answers a link to hand the person. They open it signed in
and drop the files in, `list_uploads` says what landed, and the next deploy declares those hashes in
`assets`.

That is true however big the files are, and it is not a reason to run the CLI.

## When to reach for the CLI

One case: a project that runs a framework build. The `wawesome-cli` skill carries the trigger and
the steps.

Everything else is this endpoint. Reading, deploying code and static files, invoking, fetching,
rolling back, domains, environment variables. One round trip each, structured answers, no shell.

## Prove it before saying it works

`deploy_function` answers `accepted` with a `deploy_id` and holds nothing open. `deploy_status` says
how the deploy went and where the Function answers.

Then `fetch_function` asks the live Version for one path and answers the bytes it served. Use it.
Never tell somebody a page works on the strength of a deploy having been accepted, and never ask
them to open a browser and describe what they got.

## A deploy declares the whole Version

Nothing is inherited from the Version before it. A deploy that leaves `assets` out takes every file
off the live site, which is why it is refused without `confirm_asset_drop`. Keeping the files means
declaring them again, and `get_function_source` answers the hash of each one, so nothing is
uploaded twice.

Rolling back moves the pages and the handler together, because they are one Version.

## What only a person can do

`whoami` and `list_apps` answer a `dashboard_url`. That is where the work no credential covers gets
done.

- **Real secrets.** Never ask for one in this conversation. Whatever `set_env_var` writes has
  travelled through the transcript and whatever the chat product keeps of it. That is fine for a
  feature flag and wrong for an API key. A real one is set by the person who holds it at
  `{dashboard_url}?tab=secrets`.
- **Outbound network.** Refused by default. A Function that fetches another host needs that host
  allowed at `{dashboard_url}?tab=egress`, and only a person can widen it.
- **Widening this connection.** A capability refusal is settled by connecting again and approving a
  wider job. See the `wawesome-setup` skill.
- **Detaching a domain, billing and the plan.** All of them, on the dashboard.

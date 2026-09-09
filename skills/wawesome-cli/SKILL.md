---
name: wawesome-cli
description: Use when deploying a wawesome project that runs a framework build, meaning one where a command such as `vite build`, `astro build`, `next build` or `npm run build` has to run before there is anything to deploy. wawesome runs no build, so that build runs on this machine and the CLI uploads what it wrote. Nothing else needs the CLI. Reading, deploying hand-written code and static files, invoking, fetching, rolling back, domains and environment variables all go through the wawesome MCP tools, and files on the person's own machine go through `request_upload`.
---

# Deploying a framework build with the CLI

The platform runs no build. A deploy serves the files it declared. So a project whose files do not
exist until a build has run has to be built here, and the CLI is what runs it and sends the result.

That is the whole of the trigger. Size, file count, a preference for a terminal and files sitting on
somebody's disk are all not it.

## What it needs

Node 22.13 or newer, and nothing else. `npx` fetches the CLI each time it is named.

```bash
npx wawesome login
```

Skip this if `npx wawesome whoami` already answers. The `wawesome-setup` skill covers the sign-in.

## Declare the build

`wawesome-function.json` sits in the project root and says what to build and what the build wrote:

```json
{
  "app": "my-site",
  "function": "root",
  "build": "vite build",
  "entry": "dist/server/index.js",
  "assets": "dist/client"
}
```

`build` runs before anything is read, so `entry` and `assets` name what it produced rather than what
is in the repository now.

Name the Function `root`. Its files then answer at the App's own address, so markup linking to
`/about` or a stylesheet at `/style.css` resolves as written and the build needs no rewriting. Any
other name puts the Function one segment down, at `/{function_name}`, and those links break.

A build that produces pages and no server leaves `entry` out. The deploy then carries the site and
nothing runs.

`npx wawesome init --template react-ssr` scaffolds a working example of the whole thing.

## Deploy

```bash
npx wawesome deploy
```

It runs the build, hashes every file, sends only the bytes the platform does not already hold, and
promotes the new Version. It prints the address, the version number and the visibility.

If it fails, what it prints is the gateway's own error.

## Then go back to the tools

The CLI ran the build. It is not where the rest of the work belongs. Check the result with
`fetch_function`, read what happened with `read_invocation_logs`, and undo it with
`rollback_function`.

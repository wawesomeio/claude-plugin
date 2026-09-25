# Changelog

## 0.1.0 (2026-09-25)

The first release.

- The MCP endpoint at `https://api.wawesome.io/v1/mcp`. You sign in on the first tool call, in the browser.
- Three skills:
  - `wawesome-setup`: how to connect, which job on the consent screen carries which tools, and what to do when a tool refuses.
  - `wawesome-platform`: what a deploy from Claude Code can carry, how to check what is live, and the work only a person can do on the dashboard.
  - `wawesome-cli`: how to deploy a project that runs a framework build.
- Two commands:
  - `/wawesome-deploy` deploys this project and fetches the result back.
  - `/wawesome-diagnose` finds out why a Function is failing.

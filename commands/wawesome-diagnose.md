---
description: Find out why a wawesome Function is failing
argument-hint: "[function name or invocation id]"
allowed-tools: [Read, Glob, Grep, Bash]
---

Work out why a wawesome Function is failing.

$ARGUMENTS names the Function, or one invocation id, if the user gave either.

1. Start with `diagnose_latest_failure`. It answers the failure and the source together.
2. Given an invocation id, read that one run with `get_invocation` and `read_invocation_logs`.
   Otherwise `list_invocations` lists the recent runs and how each ended.
3. Read the code with `get_function_source`. The live Version is the thing that failed, whatever the
   files in this directory say.
4. A handler that fails while fetching another host is usually the egress allowlist. Outbound
   network is refused by default and only a person can widen it, at
   `{dashboard_url}?tab=egress`.
5. Reproduce it: `fetch_function` for a page, `invoke_function` for a handler.
6. Report the cause and the fix. Deploy only if the user asks for it.

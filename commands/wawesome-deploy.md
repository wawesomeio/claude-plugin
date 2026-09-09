---
description: Deploy this project to wawesome and fetch the result back
argument-hint: "[function name]"
---

Deploy this project to wawesome, then prove it answers.

$ARGUMENTS names the Function, if the user gave a name.

1. Call `whoami`. If no wawesome tool is there, or it refuses, follow the `wawesome-setup` skill and
   stop.
2. Work out what this project is. One that runs a framework build goes through the `wawesome-cli`
   skill. Everything else deploys from here.
3. Read what is live first, with `list_functions` and `get_function_source`. A Function that already
   exists is being edited, not replaced.
4. Deploy with `deploy_function`. Use the name the user gave, else the name already live, else
   `root` for anything whose markup links start at `/`.
5. Poll `deploy_status` with the `deploy_id` it answered.
6. Fetch the address back with `fetch_function` and report the bytes it served. Do not call it done
   before this.

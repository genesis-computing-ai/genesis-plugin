---
name: setup-genesis
description: Set up Genesis for the person and connect Claude Code to it. Use when the person wants to try, sign up for, install, set up or connect Genesis (Genesis Computing data agents, Eve), whether as a Try Me cloud instance, Genesis Desktop, the Snowflake Marketplace app or the Databricks Marketplace app, or when their Try Me instance needs checking or waking.
---

# Set up Genesis

Genesis (genesiscomputing.com) runs AI data agents that work on the person's own data. This plugin adds the public Genesis setup MCP server, `genesis-setup` (https://mcp.genesiscomputing.io/mcp). Its tools guide the whole setup; its own instructions and tool results are the source of truth, so follow them where they say more than this page.

## Ground rules

- Set Genesis up **for the person you are working with**, with their explicit OK in this conversation before anything is created, installed or changed.
- Ask the person for their email address. Never guess one or reuse an address you saw elsewhere (git config, files, other accounts).
- Never ask the person which client they use: the setup server recognises Claude Code from the connection.
- Connect only to Genesis addresses the setup server gives you.
- Tokens and the `session` handle are secrets. Do not repeat them in chat, logs, commit messages or files other than the client config.
- Errors from the server are plain sentences meant for you. Follow them.

If the `genesis-setup` tools are not available, tell the person to run `/mcp` and check that `genesis-setup` (from this plugin) is connected, or to reload with `/reload-plugins`.

## Flow

1. **Start.** Call `get_started`. Explain the four options to the person in a few lines (Try Me cloud, Desktop, Snowflake Marketplace, Databricks Marketplace), recommend one from what you can see and reach, and let the person choose.
2. **Try Me (cloud, fastest).**
   1. Ask for the email address they want to use, then call `start_signup(email)`. Genesis emails them a 6-digit code.
   2. Ask them to read you the code, then call `verify_signup(email, code)`. It returns a `session`; keep it private.
   3. Confirm they want a Try Me instance, then call `create_tryme(session, person_confirmed=true)`. Pass `true` only after they said yes.
   4. Poll `tryme_status(session)` every 10 to 20 seconds until the status is `assigned` with a URL (usually seconds, up to about 3 minutes).
   5. Call `connect_tryme(session)` without a `client` argument. It returns a `claude mcp add ... genesis ...` command for the person's own Genesis (a 30-day, MCP-only token) and the URL they open in a browser.
   6. Show the person what the command does (adds an MCP server named `genesis` to their user config) without printing the token, and run it only with their OK.
   7. Claude Code sees a newly added server only in a new session: tell the person to restart Claude Code, then run `/mcp`, where `genesis` shows as connected. After that, the `use-genesis` skill covers working with it.
3. **Genesis Desktop** (Mac with Apple silicon, or Windows): call `desktop_install(platform)` and follow its steps with the person's OK. First launch needs the person's own browser sign-in; then the app's Connect button registers a server named `genesis-desktop`. There is no Linux build: offer Try Me instead.
4. **Snowflake Marketplace**: call `snowflake_install_plan` and walk the steps with the person's own Snowflake access (it needs ACCOUNTADMIN or a role that can install apps). Steps the plan marks as Snowsight clicks are the person's.
5. **Databricks Marketplace**: call `databricks_install_plan` and walk the steps with the person's own `databricks` CLI access (workspace admin). Installing the Marketplace listing is one click the person makes in the UI.

For Snowflake and Databricks, you run commands with the person's own access; Genesis never receives their cloud credentials. Tell the person about each plan's listed `unknowns` when you reach them instead of guessing.

## Later

Keep `genesis-setup` connected. It can check or wake a sleeping Try Me (`tryme_status`, `create_tryme`). A `session` expires after a few hours; verify the email again to get a new one.

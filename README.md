# Genesis plugin for Claude Code

Set up [Genesis](https://genesiscomputing.com) data agents and work with them from Claude Code. Claude walks you through signing up (or installing Genesis in your own Snowflake or Databricks account), connects itself to your Genesis, and then hands data work to Genesis agents such as Eve.

## Install

In Claude Code, run:

```text
/plugin marketplace add genesis-computing-ai/genesis-plugin
/plugin install genesis@genesis
```

The second command opens the plugin's details: choose a scope (**Install for you** is the usual choice). From your shell instead of a session:

```bash
claude plugin marketplace add genesis-computing-ai/genesis-plugin
claude plugin install genesis@genesis
```

Then ask Claude something like *"Set up Genesis for me"*, or run `/genesis:setup-genesis`.

## What it installs

| Component | What it does |
|---|---|
| `genesis-setup` MCP server | The public Genesis setup server at `https://mcp.genesiscomputing.io/mcp`. No account or sign-in needed to connect. It explains the ways to run Genesis, signs you up with a code sent to your email, starts a free Try Me instance, and gives Claude the command to connect to it. |
| `setup-genesis` skill | Walks the setup flow: Try Me (cloud), Genesis Desktop, Snowflake Marketplace or Databricks Marketplace. Claude asks for your email and your OK before creating or installing anything. |
| `use-genesis` skill | Once your own Genesis is connected (as the `genesis` MCP server), shows Claude how to find its tools and hand work to Genesis agents. |

Your own Genesis is added as a separate MCP server named `genesis` during setup, with your OK. Claude Code picks it up in a new session: restart Claude Code, then run `/mcp` to see it connected.

Already have a Genesis server? Ask Claude to use it: the `use-genesis` skill works with any connected Genesis. To add one by hand, open **Config > MCP Gateway** in your Genesis for the exact command, or see [Connect coding agents over MCP](https://docs.genesiscomputing.com).

## Updates

Claude Code does not auto-update third-party marketplaces by default. To turn it on: run `/plugin`, open the **Marketplaces** tab, select **genesis**, and turn on auto-update. To update by hand, run `claude plugin update genesis@genesis` in your shell, or choose **Update now** on the plugin in the **Installed** tab of `/plugin`.

Maintainers: users receive a new copy only when `version` in `.claude-plugin/plugin.json` changes, so bump it with every release.

## Privacy

The setup server sees the email address you give it and the sign-up code you read back. The token that connects Claude Code to your Genesis is stored in your Claude Code MCP configuration; it is MCP-only, expires after 30 days, and can be revoked under **API Keys** in Genesis. For Snowflake and Databricks installs, Claude runs commands with your own access; Genesis never receives your cloud credentials.

## Support

- Docs: https://docs.genesiscomputing.com
- Email: support@genesiscomputing.ai

## License

Apache-2.0. See [LICENSE](LICENSE).

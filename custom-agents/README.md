# Custom Agents

Custom agent artifacts are in alpha and require Tempo to be enabled for the tenant. Contact your Skedulo Customer Success Manager to request access.

If `sked artifacts custom-agent` is missing from your CLI, run `sked artifacts update-descriptors` to refresh the list of artifact types.

A custom agent is an agent profile: a stock persona (`template`) plus structured `instructions` and an allowlist of `tools`. Deploying saves the profile as a draft and publishes it in one step. `name` is lowercase kebab-case and `system` is reserved.

| File | What it shows |
| --- | --- |
| `dispatch-assistant.custom-agent.json` | The minimal profile: a scheduler persona with read, suggest, and allocate tools. |
| `parts-availability.custom-agent.json` | The `external_api` tool with `allowedDomains` restricting the hosts the agent may call. |

## Pairing a secret with a host

An `allowedDomains` entry can list configuration variable keys the agent may send to that host as `{{secret:KEY}}`:

```json
"allowedDomains": [
  {
    "domain": "parts.example.com",
    "secrets": ["PARTS_SUPPLIER_API_KEY"]
  }
]
```

`PARTS_SUPPLIER_API_KEY` is the `config-variables/PARTS_SUPPLIER_API_KEY.config-variable.json` artifact. When both are shipped in a package, configuration variables deploy before custom agents, so the secret exists by the time the agent is published. Deploy them in the same order when deploying by hand:

```
sked artifacts config-variable upsert -f config-variables/PARTS_SUPPLIER_API_KEY.config-variable.json -a <alias>
sked artifacts custom-agent upsert -f custom-agents/parts-availability.custom-agent.json -a <alias>
```

A host that carries a secret must also be on the platform-curated destination list, or publishing fails with `allowed_domain_not_curated`. The committed example omits `secrets` so that it deploys as-is; ask Skedulo to curate your supplier's host before adding them.

Documentation:

* [Custom agents](https://docs.skedulo.com/developer-guides/tempo/custom-agents/)
* [sked artifacts custom-agent](https://docs.skedulo.com/developer-guides/cli/command-reference/artifacts/custom-agent/)

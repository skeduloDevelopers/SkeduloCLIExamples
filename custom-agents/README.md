# Custom Agents

Custom agent artifacts are in alpha and require Tempo to be enabled for the tenant. Contact your Skedulo Customer Success Manager to request access.

If `sked artifacts custom-agent` is missing from your CLI, run `sked artifacts update-descriptors` to refresh the list of artifact types.

A custom agent is an agent profile: a stock persona (`template`) plus structured `instructions` and an allowlist of `tools`. Deploying saves the profile as a draft and publishes it in one step. `name` is lowercase kebab-case and `system` is reserved.

| File | What it shows |
| --- | --- |
| `dispatch-assistant.custom-agent.json` | The minimal profile, on the `scheduler` template. |
| `skedulo-api-assistant.custom-agent.json` | The `generalist` template, with the `skedulo_api` tool. |
| `parts-availability.custom-agent.json` | The `external_api` tool with `allowedDomains` restricting the hosts the agent may call. |

An `allowedDomains` entry can also list configuration variable keys the agent may send to that host as `{{secret:KEY}}`:

```json
"allowedDomains": [
  {
    "domain": "parts.example.com",
    "secrets": ["PARTS_SUPPLIER_API_KEY"]
  }
]
```

Deploy the configuration variable before the agent. A host that carries a secret must also be on the platform-curated destination list, or publishing fails with `allowed_domain_not_curated`, so the committed example omits `secrets`.

For these agents in use, with prompts to try, see [Tempo Agent Examples](https://github.com/skeduloDevelopers/TempoAgentExamples).

Documentation:

* [Custom agents](https://docs.skedulo.com/developer-guides/tempo/custom-agents/)
* [sked artifacts custom-agent](https://docs.skedulo.com/developer-guides/cli/command-reference/artifacts/custom-agent/)

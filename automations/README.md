# Automations

Automation artifacts are in alpha and are available on the Skedulo Pulse Platform only. The automations beta must be switched on for your team, and the AI actions used here only appear once Tempo is enabled for the tenant. Contact your Skedulo Customer Success Manager to request access.

If `sked artifacts automation` is missing from your CLI, run `sked artifacts update-descriptors` to refresh the list of artifact types.

Each file holds a trigger and a workflow. The workflow is an Amazon States Language state machine in which every `Task` state names an action as `action:<action-name>` and passes its arguments in `Arguments`, with JSONata expressions in `{% %}`. A step's output is available to later steps under a variable named after the step, for example `$createTempoTask.result.data.taskId`.

| File | Trigger | What it does |
| --- | --- | --- |
| `sick-leave-find-cover.automation.json` | A sick-leave availability is inserted | Creates a high-priority Tempo task with the leave and the resource attached, then runs it so Tempo proposes cover. |
| `customer-cancellation-refill.automation.json` | A job's status changes to Cancelled | Creates a Tempo task with the job attached, then runs it so Tempo offers the freed slot to pending work. |

Deploy with:

```
sked artifacts automation upsert -f automations/sick-leave-find-cover.automation.json -a <alias>
```

Documentation:

* [Create Tempo tasks from automations](https://docs.skedulo.com/developer-guides/tempo/create-tasks-from-automations/)
* [sked artifacts automation](https://docs.skedulo.com/developer-guides/cli/command-reference/artifacts/automation/)
* [Introduction to automations](https://docs.skedulo.com/user-guides/admin-and-config/admin-and-config-webapp/process-automation/automations/introduction-to-automations/)

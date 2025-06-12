# Conjur Policy Guide: `!layer` Statement

## Overview

The `!layer` statement defines a **collection of hosts** that can be managed as a single unit. Layers are often used to group machine identities (hosts) that need similar access rights to secrets.

While similar to groups, which can be used for users, **layers are specifically for hosts**. This is especially useful when managing infrastructure at scale, such as applying access controls to an entire fleet of CI/CD jobs, containers, or applications.

---

## When to Use `!layer`

Use `!layer` when you want to:

- Group multiple hosts together under a single role
- Apply consistent access rights to a fleet of machines
- Keep your host management scalable and maintainable

---

## Example

```yaml
- !layer dev-app-hosts
```

This creates a layer called `dev-app-hosts`. Hosts can be added to the layer using `!grant`:

```yaml
- !grant
  role: !layer dev-app-hosts
  member: !host frontend-runner-01
```

You can then permit the layer access to a resource:

```yaml
- !permit
  role: !layer dev-app-hosts
  privileges: [read, execute]
  resource: !variable app/secret_token
```

---

## How It Works

Layers act as **host groupings**. Any host added to a layer inherits the permissions assigned to that layer.

Use layers instead of assigning individual `!permit` statements for each host — this reduces policy clutter and makes it easier to manage machine identity access over time.

---

## Best Practices

- Use layers to manage access by environment or function (e.g., `ci-hosts`, `staging-apps`, `production-services`)
- Store layer declarations alongside host and permission policies for visibility
- Grant access to layers instead of individual hosts whenever possible to improve maintainability
- Layer names should reflect the group’s purpose clearly

---

## Applying a Layer Policy

To apply a policy with a `!layer` declaration:

```bash
conjur policy load -b app -f layers.yml
```

---

## Additional Resources

- [Official Documentation – `!layer`](https://docs.cyberark.com/conjur-cloud/latest/en/content/operations/policy/statement-ref-layer.htm)

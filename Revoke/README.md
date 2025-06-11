# Conjur Policy Guide: `!revoke` Statement

## Overview

The `!revoke` statement is used to **remove a previously granted role membership**. This is the inverse of a `!grant` statement and is useful for managing access lifecycle — especially when automating user, host, or group offboarding.

Use `!revoke` when you need to ensure a role no longer has access via group or layer membership.

---

## When to Use `!revoke`

Use `!revoke` when you want to:

- Remove a user or host from a group or layer
- Reverse a previous `!grant` operation
- Clean up access after automation jobs, team changes, or policy updates

---

## Example

```yaml
- !revoke
  role: !group app-deployers
  member: !host ci-github-actions-runner
```

### What this does:

- Removes the `ci-github-actions-runner` host from the `app-deployers` group
- This revokes any privileges the host inherited from that group

---

## How It Works

The `!revoke` statement removes a **membership relationship** between a role and a group or layer. Once revoked, the member will no longer inherit any permissions from that group or layer unless other grants still apply.

`!revoke` only removes group/layer membership — it does **not** remove direct permissions (defined via `!permit`) or delete the role or resource.

---

## Best Practices

- Use `!revoke` to manage short-lived access for CI/CD pipelines or contractors
- Keep `!revoke` policies stored alongside related `!grant` or `!group` policies for auditability
- Regularly review and rotate access using automated revocation as part of your access governance
- Always use `conjur policy update` when applying a policy containing `!revoke`

---

## Applying a Revoke Policy

To apply a policy with a `!revoke` statement:

```bash
conjur policy update -b app -f revoke-access.yml
```

This will remove the specified role membership from the `app` policy branch.

---

## Additional Resources

- 📖 [Official Documentation – `!revoke`](https://docs.cyberark.com/conjur-cloud/latest/en/content/operations/policy/statement-ref-revoke.htm)

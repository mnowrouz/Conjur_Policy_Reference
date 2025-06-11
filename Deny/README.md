# Conjur Policy Guide: `!deny` Statement

## Overview

The `!deny` statement in Conjur is used to explicitly **block a role from performing specific actions** on a resource. It overrides any previously granted permissions from `!permit` statements, which is useful for enforcing strict access controls or revoking permissions without deleting the role or resource.

Use this when you want to enforce a clear **"no access"** rule, even if the role is part of another group or layer that would otherwise allow it.

---

## When to Use `!deny`

Use `!deny` when you want to:

* Explicitly revoke a role's ability to perform an action (e.g., `read`, `execute`, `update`)
* Override inherited or layered permissions from `!permit` statements
* Prevent accidental access from broad group membership

---

## Example

```yaml
- !deny
  role: !user alice
  privileges: [read, execute]
  resource: webapp/secret-data
```

### What this does:

* Prevents the user `alice` from reading or executing `webapp/secret-data`
* Even if `alice` is a member of a group that has `!permit` rules for this resource, the deny will take precedence

---

## How It Works

A `!deny` statement targets:

* A specific role (usually a `!user` or `!group`)
* A set of privileges (e.g., `read`, `update`, `execute`)
* A fully-qualified resource (including its namespace, if applicable)

This statement takes effect immediately upon policy update and **overrides any previously granted access**. It is non-destructive — the role and resource are not deleted — but access is explicitly blocked.

---

## Best Practices

* Use `!deny` to enforce clear boundaries or exceptions in access policy
* Prefer `!deny` over removing a `!permit` when you want to audit or temporarily block access
* Always reference the full resource path, especially if defined in a nested policy
* Store deny policies in a clearly named file (e.g., `deny-access.yml`)
* Place deny policy files in the same folder as the policy they modify (e.g., `prod/webapp/deny-access.yml`)

---

## Applying a Deny Policy

To apply a deny policy:

```bash
conjur policy update -b root -f deny-access.yml
```

Or for a nested policy:

```bash
conjur policy update -b prod/webapp -f deny-access.yml
```

---

## Additional Resources

* 📖 [Official Documentation – `!deny`](https://docs.cyberark.com/conjur-cloud/latest/en/content/operations/policy/statement-ref-deny.htm)

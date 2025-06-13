# Conjur Policy Guide: `!policy` Statement

## Overview

The `!policy` statement defines a **nested policy** within a parent policy. Think of it as creating a namespace, similar to how Kubernetes uses namespaces to isolate and organize resources.

In Conjur, a policy acts as a **namespace-like container** for users, groups, roles, and other elements. This structure:

* Scopes all resources defined within the policy.
* Prevents naming collisions.
* Helps organize policies by application, team, or function.

This makes your secret management structure modular, clean, and maintainable.

---

## When to Use `!policy`

Use `!policy` when you want to:

* Create a logical namespace (e.g., for an application or team)
* Organize related policy elements in a clean, readable way
* Break policies into smaller, modular files
* Improve long-term maintainability

---

## Example

```yaml
- !policy
  id: webapp
  owner: !group security-admins
  annotations:
    authn/api-key: true
  body:
    - !host backend

```

### What this does:

* Creates a sub-policy named `webapp`
* Makes `security-admins` the owner of the new policy
* Creates a host called backend

Once loaded, the host resource can be referenced as:

* `webapp/host`

---

## How It Works

The `!policy` block creates a new namespace under the current policy. All items inside `body` live within that namespace.

When you reference these resources elsewhere (e.g., in a `!permit` rule), use the fully-qualified name:

```yaml
!group: webapp/app-users
```

This keeps things organized and avoids naming conflicts across your Conjur policy ecosystem.

---

## Best Practices

*  Use short, meaningful policy IDs (e.g., `webapp`, `infra`, `ci`)
*  Start big and work your way down - e.g. load your app policy under your business unit policy, then load your environment policy under your app policy.
*  Keep each nested policy small and focused
*  Avoid deeply nested policies unless necessary
*  Use version control for all your policy files
*  Store each policy file in a folder that matches the policy it defines. For example, a policy named webapp should be saved in data/webapp/webapp.yml to reflect its location in the policy tree.

---

## Loading a Policy

To load this policy:

```bash
conjur policy load -b root -f webapp.yml
```

This creates a `webapp` namespace under the root policy.

---


## Additional Resources

* [Official Documentation – `!policy`](https://docs.cyberark.com/conjur-cloud/latest/en/content/operations/policy/statement-ref-policy.htm)

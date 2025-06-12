# Conjur Policy Guide: `!grant` Statement

## Overview

The `!grant` statement in Conjur is used to **assign a role as a member of another role**. Most commonly, it is used to add users to groups or layers. This simplifies permission management by allowing access to be granted at the group level instead of per user.

When a user is granted membership in a group, they inherit all privileges that have been permitted to that group.

---

## When to Use `!grant`

Use `!grant` when you want to:

* Add a user to a group (e.g., `alice` → `app-users`)
* Give a host access to secrets via the Safe’s consumers group
* Give a host permission to authenticate to Conjur using a defined authenticator

---

## Example

```yaml
- !grant
  role: !group app-users
  member: !user alice
```

### What this does:

* Adds the user `alice` to the group `app-users`
* `alice` will inherit all permissions that have been granted to `app-users`

---

## How It Works

A `!grant` statement connects two roles:

* The `role` is the destination role (usually a `!group` or `!layer`)
* The `member` is the role being added (usually a `!user` or another `!group`)

Once granted, the member inherits all permissions associated with the parent role. This inheritance is immediate and automatically updated when policy is applied.

---

## Best Practices

* Use `!grant` to group users by access level or team function
* Keep group names meaningful to reflect their purpose (e.g., `app-admins`, `ci-users`)
* Always reference the full role path when working in nested policies
* Store grant statements in a dedicated file (e.g., `grant-users.yml`)
* Place grant files in the same folder as the policy they modify (e.g., `policies/webapp/grant-users.yml`)

---

## Applying a Grant Policy

To apply a grant policy:

```bash
conjur policy load -b root -f grant-users.yml
```

Or for a nested policy:

```bash
conjur policy load -b dev/webapp -f grant-users.yml
```

---

## Additional Resources

* [Official Documentation – `!grant`](https://docs.cyberark.com/conjur-cloud/latest/en/content/operations/policy/statement-ref-grant.htm)

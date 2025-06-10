# Conjur Policy Guide: `!delete` Statement

## Overview

The `!delete` statement in Conjur is used to **remove a resource** from the policy tree. This allows you to clean up resources like users, groups, variables, or even entire nested policies when they are no longer needed.

**This is a destructive action.** Once a resource is deleted using `!delete`, it is permanently removed from Conjur and cannot be recovered through policy. Always take caution and ensure the resource is no longer in use before proceeding.

---

## When to Use `!delete`

Use `!delete` when you want to:

* Remove unused or deprecated users, groups, or roles
* Delete test policies or cleanup development environments
* Simplify or refactor your policy structure

---

## Example

```yaml
- !delete
  record: !user alice
```

### What this does:

* Deletes the user `alice` from Conjur
* If any roles or permissions were granted to `alice`, they will no longer be effective

---

## How It Works

The `!delete` statement targets a specific resource by type and ID. You must specify both:

* The kind of resource (e.g., `!user`, `!group`, `!policy`, etc.)
* The fully-qualified name (e.g., `webapp/alice`)

If you defined the user under a nested policy, the resource name must include that namespace:

```yaml
- !delete
  record: !user webapp/alice
```

This ensures that the correct resource in the correct namespace is deleted.

---

## Best Practices

* Use `!delete` only in controlled environments where changes are reviewed
* Always check for references or dependencies before deleting a resource
* Track delete operations in source control alongside your policy history
* Store delete policies in clearly named files (e.g., `delete-users.yml`)
* Keep delete files in the same folder as the policy they modify (e.g., `policies/webapp/delete-users.yml`)

---

## Deleting a Resource

To apply a deletion policy:

```bash
conjur policy update -b root -f delete-users.yml
```

Or to delete from a nested policy:

```bash
conjur policy update -b webapp -f delete-users.yml
```

---

## Additional Resources

* 📖 [Official Documentation – `!delete`](https://docs.cyberark.com/conjur-cloud/latest/en/content/operations/policy/statement-ref-delete.htm)

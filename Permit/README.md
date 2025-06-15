# Conjur Policy Guide: `!permit` Statement

## Overview

The `!permit` statement is used to explicitly **grant access** to a role. In Conjur, all access must be explicitly granted — there are no default permissions. This makes `!permit` the core mechanism for authorizing actions on resources like variables, hosts, and other roles.

---

## When to Use `!permit`

Use `!permit` when you want to:

- Grant a user, host, or group permission to access a resource
- Allow a host (i.e. CI/CD pipeline) to retrieve a secret
- Delegate permission to execute a service or command in a controlled way

---

## Example

```yaml
- !permit
  role: !group app-deployers
  privileges: [read, execute]
  resource: !variable db/password
```

### What this does:

- Grants the `app-deployers` group permission to **read** and **execute** the `db/password` variable
- Any users or hosts that are members of `app-deployers` will inherit these permissions

---

## How It Works

The `!permit` statement defines an **access rule** between:

- A **role** (user, host, group, or layer)
- One or more **privileges** (`read`, `execute`, `update`, `create`, etc.)
- A **resource** (typically a variable or other object)

## Predefined Privilege Values

### Privileges for `!variable` resources

| Privilege  | Description                                                                 |
|------------|-----------------------------------------------------------------------------|
| `read`     | Allows reading metadata about the variable (e.g., ID, annotations).         |
| `execute`  | Allows retrieving the secret value of the variable.                         |
| `update`   | Allows changing the variable's value.                                       |

### Privileges for other resource types (e.g., layers, groups, webservices)

| Privilege  | Description                                                                       |
|------------|-----------------------------------------------------------------------------------|
| `read`     | Allows reading metadata about the resource.                                       |
| `update`   | Allows modifying the resource or its metadata.                                    |
| `create`   | Allows creating new resources within a policy namespace. (applies only to policy) |
| `use`      | Allows a role to create a dynamic resource from an issuer.                        |

---

## Best Practices

- Use group or layer roles in `!permit` statements to avoid granting permissions directly to individual users or hosts
- Keep permission grants close to the resource definition in your policy structure
- Use least privilege: grant only the minimum required privileges to each role
- Store `!permit` statements in the same policy folder as the resource they apply to (e.g., in `prod/app/permissions.yml`)
- Prefer fully qualified resource names if the resource is defined in a different policy namespace

---

## Applying a Permit Policy

To apply a policy with a `!permit` statement:

```bash
conjur policy load -b app -f permissions.yml
```

This will add the access rule(s) to the specified policy branch (`app` in this case).

---

## Additional Resources

- [Official Documentation – `!permit`](https://docs.cyberark.com/conjur-cloud/latest/en/content/operations/policy/statement-ref-permit.htm)

# Conjur Policy Guide: `!variable` Statement

## Overview

The `!variable` statement defines a **secret resource** in Conjur. Variables are the objects that hold the secret values (such as API keys, database credentials, or access tokens).

However, in most real-world deployments of Conjur, variable creation directly in policy is considered a **fringe use case**. Variables are typically **synchronized** into Conjur from **CyberArk Privilege Cloud** or **CyberArk PAM Self-Hosted** via the Conjur Synchronizer process.

---

## When to Use `!variable`

Use `!variable` when you:

- Are setting required values for authenticators
- Are working in a test, development, or isolated environment
- Need to define a variable for a proof-of-concept or demo
- Are managing secrets directly in Conjur without CyberArk integrations (not recommended in production)

---

## Example

```yaml
- !variable password
```

### What this does:

- Declares a variable resource called `password`
- This variable can later be assigned a value and accessed via permissions granted with `!permit`

---

## How It Works

In Conjur, variables represent named secrets. Once a `!variable` is declared, it must be:

1. Assigned a value (e.g., using `conjur variable set -i password -v Cyberark1`)
2. Accessed via authorized roles (e.g., hosts or users granted `read` and `execute` privileges)

If a variable is declared in policy but no value is assigned or no access is granted, it remains inert.

---

## Managed vs Unmanaged Variables

**Managed variables** are provisioned and maintained via integrations such as:

- CyberArk Privilege Cloud
- CyberArk PAM Self-Hosted

Both via the Conjur Synchronizer and the ConjurSync user for Privilege Cloud

These provide centralized management, credential rotation, and access control — all synchronized automatically into Conjur.

**Unmanaged variables** (those created natively via `!variable`) are not recommended in production:

- No automatic rotation or lifecycle management
- No centralized governance
- Risk of configuration drift or orphaned secrets

---

## Best Practices

- Avoid creating `!variable` entries directly in policy unless testing or prototyping
- Use CyberArk integrations to manage secrets in Conjur for production use
- If you do define native variables, store them in a clearly named policy file and grant access explicitly
- Pair all variable declarations with permissions (`!permit`) and avoid unused variables

---

## Applying a Variable Policy

To apply a policy containing variable declarations:

```bash
conjur policy load -b dev/app -f variables.yml
```

---

## Additional Resources

- [Official Documentation – `!variable`](https://docs.cyberark.com/conjur-cloud/latest/en/content/operations/policy/statement-ref-variable.htm)
- [CyberArk Synchronizer Overview](https://docs.cyberark.com/ConjurCloud/Latest/en/Content/Integrations/cybr-pcloud-sync.htm)

# Conjur Policy Guide: `!webservice` Statement

## Overview

The `!webservice` statement is used to define a **logical Conjur web service endpoint**. Webservices are used internally by Conjur for authentication workflows, particularly with external authenticators such as `authn-jwt`, `authn-k8s`, and `authn-oidc`.

While most Conjur users won’t need to create `!webservice` objects directly, they are essential when defining **custom authenticators** or creating policies that integrate with Conjur’s built-in authentication framework.

---

## When to Use `!webservice`

Use `!webservice` when you:

- Are defining a custom authenticator or implementing a custom authentication plugin
- Need to explicitly control or delegate ownership of an authentication webservice
- Are creating a policy that needs to reference a specific authenticator identity

In typical usage, Conjur creates these automatically behind the scenes for standard authenticators (e.g., `authn`, `authn-jwt`, `authn-k8s`), so manual declaration is rare.

---

## Example

```yaml
- !webservice
  id: authn-jwt/dev
```

### What this does:

- Declares a webservice with the ID `authn-jwt/dev`
- This might represent a JWT authenticator integration configured under the `dev` environment
- This webservice can then be referenced in policies that involve JWT authentication or auditing

---

## How It Works

Each authenticator integration in Conjur is backed by a webservice ID. For example:

- The JWT authenticator at `authn-jwt/dev` is a webservice
- Kubernetes authentication at `authn-k8s/dev` is also a webservice

These webservices can have roles (like users or hosts) authenticate **against them**, and `!permit` or `!deny` statements can be used to control access to the webservice.

---

## Best Practices

- Only define `!webservice` manually if you're creating a custom authentication plugin or need explicit control
- Keep naming consistent with your authenticator hierarchy (e.g., `authn-jwt/dev`, `authn-oidc/ci`)
- Store the declaration in the same policy file where the authenticator integration is configured
- Avoid granting broad access to webservices — use fine-grained `!permit` where necessary

---

## Applying a Webservice Policy

To apply a policy with a `!webservice` declaration:

```bash
conjur policy load -b root -f webservice.yml
```

Or to a sub-policy branch:

```bash
conjur policy load -b authn-jwt -f dev-webservice.yml
```

---

## Additional Resources

- 📖 [Official Documentation – `!webservice`](https://docs.cyberark.com/conjur-cloud/latest/en/content/operations/policy/statement-ref-webservice.htm)
- 📖 [Conjur JWT Authenticator Overview](https://docs.cyberark.com/ConjurCloud/Latest/en/Content/Integrations/JWT/jwt-authn-overview.htm)

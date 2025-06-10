# Conjur Policy Guide: `!host` Statement

## Overview

The `!host` statement in Conjur is used to define a **machine identity**. Hosts are often used to represent applications, containers, CI/CD tools, or any non-human entities that need to authenticate and access secrets.

Unlike `!user`, which is intended for human access, `!host` is optimized for automated access by machines or services. Hosts are commonly used with API keys or JWTs for authentication.

---

## When to Use `!host`

Use `!host` when you want to:

* Define a non-human identity for authentication
* Enable CI/CD pipelines, apps, or containers to retrieve secrets
* Use JWT-based or API key-based authentication

---

## Example

```yaml
- !host ci-github-actions-runner
```

### What this does:

* Declares a host identity named `ci-github-actions-runner`
* This host can later be granted access to variables and added to groups or layers

---

## How It Works

A host is treated like a role. You can:

* Grant it membership in groups or layers using `!grant`
* Assign it permissions using `!permit`
* Authenticate it using Conjur-supported methods like API key, JWT, or mutual TLS

Hosts make it easy to securely grant automated services access to secrets without assigning user credentials.

---

## JWT Authentication

JWT (JSON Web Token) authentication is a common and scalable way to authenticate hosts. When using JWT:

* Hosts are declared in policy using `!host`
* Authentication is handled via a trusted identity provider and a JWT token
* The `identity-path` in the JWT authenticator configuration refers to the **policy path** where the host resides (e.g., `app` if the host is declared under `app/frontend-service`)
* Host annotations define which claim values the authenticator should match against

### Example JWT Host Declaration

```yaml
- !host
  id: Secure_Pipeline
  annotations:
    authn-jwt/jenkins/jenkins_pronoun: Pipeline
    authn-jwt/jenkins/jenkins_task_noun: Build
```

In this case:

* The host ID is `Secure_Pipeline`, defined in the current policy path
* The JWT authenticator is located at `authn-jwt/jenkins`
* The host annotations map JWT claims like `jenkins_pronoun` and `jenkins_task_noun` to the values `Pipeline` and `Build`, respectively

This enables secure, claim-based machine identity authentication using signed JWTs and allows Conjur to validate the token against these claim values.

---

## Best Practices

* Use descriptive host IDs that reflect their environment and function (e.g., `ci-github-runner`, `app-frontend`, `pipeline-dev`)
* Avoid trailing slashes in host names unless referencing full paths from other policies
* Use folders to organize host definitions by application or environment (e.g., `policies/ci/hosts.yml`)
* Use JWT where possible for scalable, token-based host authentication
* Use host annotations to define claim-based identity matching for JWT
* Do not confuse hosts with users — hosts are for non-human identities only

---

## Applying a Host Policy

To apply a host policy:

```bash
conjur policy load -b root -f hosts.yml
```

Or for a nested policy:

```bash
conjur policy load -b app -f hosts.yml
```

---

## Additional Resources

* 📖 [Official Documentation – `!host`](https://docs.cyberark.com/conjur-cloud/latest/en/content/operations/policy/statement-ref-host.htm)
* 📖 [Using JWT Authentication in Conjur](https://docs.cyberark.com/conjur-cloud/latest/en/Content/Integrations/JWT/jwt-authn-overview.htm)

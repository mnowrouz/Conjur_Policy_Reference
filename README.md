# Conjur Policy Reference Guide

This guide gives you a central reference for working with Conjur policies—covering basic concepts, structure, syntax, and how to load them. It's designed for DevOps engineers who are starting with secrets management in Conjur.

---

## 📘 What Is a Conjur Policy?

- A **policy** is a YAML document that defines security rules and relationships between Conjur objects like users, machines, variables, and permissions.
- Think of policy as the “source code” for your access control framework in Conjur—declarative, human-readable, and auditable.
- All policy-managed objects are versioned and stored in Conjur’s database. Changes are tracked and enforceable.

---

## 🔑 Core Policy Concepts

- **Roles**: Entities that can perform actions—users, hosts, groups, or layers.
- **Resources**: Objects such as variables, hosts, and webservices.
- **Permissions**: Explicit allowances (`!permit`) for roles to act on resources—instead of default allow/deny behavior.
- **Membership**: Roles can contain other roles (using `!grant`/`!revoke`).
- **Namespaces**: Achieved through `!policy`, which scopes all contained objects under a named branch.

Conjur's model is *fully role-based*, and **no access is granted unless explicitly stated via policy**.

---

## 🗂 Policy File Structure

Conjur policy files are written in **YAML**. Key statement types include:

- `!policy` – scope/namespaces  
- `!user`, `!host`, `!group`, `!layer` – define roles  
- `!variable`, `!webservice` – define resources  
- `!grant`, `!revoke` – manage role membership  
- `!permit`, `!deny` – manage access rights

Each statement specifies an object or rule within the policy's namespace.

---

## ⚙️ Loading or Updating Policies

Use the Conjur CLI to manage your policies:

```bash
# Create or amend a policy branch
conjur policy load -b <branch-id> -f <file>.yml
```

- `<branch-id>` is the namespace (e.g., `root`, `app`, `authn-jwt`)
- Loading applies all creations, grants, permits, and revocations in the file
- “Load” is used for additive changes like `!grant`, `!permit`, etc.
- "Update" is used for both additive and destructive changes like `!revoke`, `!delete`, etc.
- For patches (like idempotent replaces), you can use flags like `--replace` (but that goes beyond the basics) - be careful using `--replace` on policies with hosts that use API keys as it will create a new instance of that host

---

## ✅ Best Practices for Policy Management

- Organize files in folders matching policy branches: each branch (e.g., `app`) should map to its own directory. Easier to track and load.
- Use modular policies with `!policy` statements to create clear namespaces.
- Store policy in version control (e.g., GitHub) and treat it like application code.
- Review changes prior to loading, to validate permissions and avoid accidental access grants.
- Manage access with least privilege: grant only what’s required, and revoke when access is no longer needed.

---

## 🔗 Useful References

- [Policy Overview](https://docs.cyberark.com/conjur-enterprise/latest/en/content/operations/policy/policy-overview.htm)
- [Policy Basic Concepts](https://docs.cyberark.com/conjur-enterprise/latest/en/content/operations/policy/policy-basic-concepts.htm)
- [Loading Policies](https://docs.cyberark.com/conjur-enterprise/latest/en/content/operations/policy/policy-load.html)
- [Policy Syntax](https://docs.cyberark.com/conjur-enterprise/latest/en/content/operations/policy/policy-syntax.htm)

---

## 🧭 What’s Next?

After digesting this primer, dive into specific statement guides below:

- [`!policy`](https://github.com/mnowrouz/Conjur_Policy_Reference/tree/main/Policy)
- [`!group`](https://github.com/mnowrouz/Conjur_Policy_Reference/tree/main/Group)
- [`!host`](https://github.com/mnowrouz/Conjur_Policy_Reference/tree/main/Host)
- [`!variable`](https://github.com/mnowrouz/Conjur_Policy_Reference/tree/main/Variable)
- [`!webservice`](https://github.com/mnowrouz/Conjur_Policy_Reference/tree/main/Webservice)
- [`!grant`](https://github.com/mnowrouz/Conjur_Policy_Reference/tree/main/Grant)
- [`!revoke`](https://github.com/mnowrouz/Conjur_Policy_Reference/tree/main/Revoke)
- [`!permit`](https://github.com/mnowrouz/Conjur_Policy_Reference/tree/main/Permit)
- [`!deny`](https://github.com/mnowrouz/Conjur_Policy_Reference/tree/main/Deny)

---

By mastering these fundamentals, you'll have full confidence structuring secure, maintainable, and auditable Conjur policies.

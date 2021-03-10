# Identity & Access Management

All the fun stuff

---

## General

[Salesforce Security Guide](https://developer.salesforce.com/docs/atlas.en-us.securityImplGuide.meta/securityImplGuide/salesforce_security_guide.htm)

- Control Who Sees What
  - Although it’s easy to confuse `permission sets` and `profiles` with `roles`, they control two different things.
  - `Permission sets` and `profiles` control a user’s **object and field access permissions**.
  - `Roles` primarily control a user’s **record-level access** through role hierarchy and sharing rules.

Access can be controlled through a variety of ways

- Profiles
- Roles
- Permission Sets
- Organization Wide Defaults (OWD)
  - Internal and external (for Experience portal users)
- Page Layouts
- Groups

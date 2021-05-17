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
- View All | Modify All permission (typically on the System Administrator Role)
- Permission Sets
- Organization Wide Defaults (OWD)
  - Internal and external (for Experience portal users)
- Field level security
- Page Layouts
- Groups
- [Queues](https://help.salesforce.com/articleView?id=sf.setting_up_queues.htm&type=5)
- Portals also have some different options?

---

## Temp - Needs Cleanup

Checking if a user has access to a record

```sql
SELECT RecordId, HasAllAccess, HasDeleteAccess, HasReadAccess, HasEditAccess, HasTransferAccess, MaxAccessLevel
FROM UserRecordAccess
WHERE UserId = '0051H00000C0v8lQAB'
AND RecordId = 'a0G1H00000tAO4wUAG'
```

See record sharing details and reasons why

- <https://SomeCompany.my.salesforce.com/p/share/CustomObjectSharingDetail?parentId=RecordIdHere>
- <https://SomeCompany.my.salesforce.com/p/share/AccSharingDetail?parentId=RecordIdHere>

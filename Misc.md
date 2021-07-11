# Miscellaneous

Miscellaneous stuff

---

## App | Package Configuration

Options for configuring apps and packages

- [Custom Settings](https://help.salesforce.com/articleView?id=cs_about.htm&type=0)
  - Can't be bundled with a package since it's not metadata
  - Custom setting records’ values can be edited in code, while custom metadata cannot
  - Supports hierarchy settings
- [Custom Metadata](https://trailhead.salesforce.com/en/content/learn/modules/custom_metadata_types_dec/cmt_overview)
  - Can be bundled with a package
- [Named Credentials](https://help.salesforce.com/articleView?id=sf.named_credentials_about.htm&type=5)
  - Setting up a Remote Site Setting is not required when using this

---

## Debugging Failed Flows

[Salesforce Debug Log Levels](https://help.salesforce.com/articleView?id=sf.code_setting_debug_log_levels.htm&type=5)

---

## Print Object Field Values

```java
Schema.DescribeSObjectResult a_desc = Account.sObjectType.getDescribe(); 
Map<String, Schema.SObjectField> a_fields = a_desc.fields.getMap();

for(Schema.sObjectField fld:a_fields.values()){ 
  system.debug(fld);
}
```

---

## Upsert Using External IDs

You can do some pretty cool stuff with upserts and external IDs.

- <https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/langCon_apex_dml_nested_object.htm>

Lets say you have a bunch of Accounts and Contacts. Instead of upserting the Accounts, then setting Contact.AccountId from the results, and then upserting the contacts... You can...

Example

```java
// Note the difference between...
// My_Account_ExternalID__c & My_Contact_ExternalID__c
// example.SomeAccountExternalId & example.SomeContactExternalId
List<Account> accounts = new List<Account>();
List<Contact> contacts = new List<Contact>();

for (SomeType example : someList) {
  Account a = new Account();
  a.My_Account_ExternalID__c = example.SomeAccountExternalId;
  a.Name = 'Example Account';
  accounts.add(a);

  Contact c = new Contact();
  c.FirstName = 'Homer';
  c.LastName = 'Simpson';
  c.My_Contact_ExternalID__c = example.SomeContactExternalId;
  // This is the magic sauce for establishing a lookup relationship using External IDs
  c.Account = new Account(My_Account_ExternalID__c=example.SomeAccountExternalId);
  contacts.add(c);
}

upsert accounts Account.My_Account_ExternalID__c;
upsert contacts Contact.My_Contact_ExternalID__c;
```

# Save Snippets

Saved Snippets of anonymous code execution and SOQL/SOSL queries

---

## SOQL

Select ID and Name for all accounts

```sql
SELECT ID, Name FROM Account
```

Select account name and contact info for linked contacts **where account name** is `ExampleAccount`

```sql
SELECT Name, (SELECT FirstName, LastName FROM Contacts)
FROM Account
WHERE Name = 'ExampleAccount'
```

Select accounts that have a linked contact with the name `Winston Henke`

```sql
-- AccountId is the lookup field name on the Contact object
Select Id, Name
From Account
Where Id IN (
  SELECT AccountId FROM Contact WHERE Name = 'Winston Henke'
  )
```

Select available Record Types for a given sObject

```sql
SELECT FIELDS(ALL)
FROM RecordType
WHERE sObjectType='Account'
LIMIT 200
```

---

Select account ID and name linked to any contacts with first/last name Winston Henke

```sql
SELECT Account.Id, Account.Name
FROM Contact 
WHERE FirstName = 'Winston' AND LastName='Henke'
```

---

## SOSL

TODO

---

## Anonymous Code Execution

TODO

```java
todo
```

---

# SOQL

Salesforce Object Query Language

---

## Examples

Get all fields

```sql
SELECT FIELDS(ALL) FROM Report LIMIT 200
```

---

TODO

- SOQL relationship query vs subquery vs aggregate query
  - <https://developer.salesforce.com/docs/atlas.en-us.soql_sosl.meta/soql_sosl/sforce_api_calls_soql_relationships_query_using.htm>
  - > Relationship queries are similar to SQL joins. However, you cannot perform arbitrary SQL joins. The relationship queries in SOQL must traverse a valid relationship path as defined in the rest of this section.

# Apex

Documentation from learning about the Apex programming language

---

## Getting Started

- [Get Started with Apex - Trailhead](https://trailhead.salesforce.com/content/learn/modules/apex_database/apex_database_intro)
- [Apex Intro - Salesforce Developer Docs](https://developer.salesforce.com/docs/atlas.en-us.224.0.apexcode.meta/apexcode/apex_intro.htm)
- Apex is a programming language that uses Java-like syntax and acts like database stored procedures
- As a language, Apex is
  - Hosted: Apex is saved, compiled, and executed on the server, the Lightning Platform
  - Object oriented: Apex supports classes, interfaces, and inheritance
  - Strongly typed: Apex validates references to objects at compile time
  - Multitenant aware: Because Apex runs in a multitenant platform, it guards closely against runaway code by enforcing limits, which prevent code from monopolizing shared resources
  - Integrated with the database: It is straightforward to access and manipulate records. Apex provides direct access to records and their fields, and provides statements and query languages to manipulate those records
    - Built in support for object triggers
  - Data focused: Apex provides transactional access to the database, allowing you to roll back operations
  - Easy to test: Apex provides built-in support for unit test creation, execution, and code coverage. Salesforce ensures that all custom Apex code works as expected by executing all unit tests prior to any platform upgrades
  - Versioned: Custom Apex code can be saved against different versions of the API
  - Case-insensitive language

---

## Random Notes

### [Collections](https://developer.salesforce.com/docs/atlas.en-us.224.0.apexcode.meta/apexcode/langCon_apex_collections.htm)

- Collections in Apex can be lists, sets, or maps
  - Lists in Apex are synonymous with arrays and the two can be used interchangeably
    - Generally, it’s easier to create a list rather than an array because lists don’t require you to determine ahead of time how many elements you need to allocate

---

### Anonymous Blocks

- An anonymous block is Apex code that does not get stored in the metadata, but that can be compiled and executed
- Good for testing/debugging Apex classes
- Compile and execute anonymous blocks using one of the following
  - Developer Console
  - Salesforce extensions for Visual Studio Code
  - The `executeAnonymous()` SOAP API call

---

### sObjects

- Every **record** in Salesforce is natively represented as an sObject in Apex
- The names of sObjects correspond to the API names of the corresponding standard or custom objects
  - API names of object and fields can differ from their labels
- For custom objects and custom fields, the API name always ends with the `__c` suffix
- For custom relationship fields, the API name ends with the `__r` suffix
- When you don’t know the type of sObject your method is handling, you can use the **generic sObject** data type
  - Variables that are declared with the generic sObject data type can reference any Salesforce record

---

### DML | Database Methods | SOQL Queries | SOSL Queries

#### Apex Data Manipulation Language(DML)

- A Object-Relational Mapping(ORM) framework similar to Entity Framework
- DML operations execute within a transaction
  - All DML operations in a transaction either complete successfully, or if an error occurs in one operation, the entire transaction is rolled back and no data is committed to the database
  - The boundary of a transaction can be a trigger, a class method, an anonymous block of code, an Apex page, or a custom Web service method. For example
    - If a trigger or class creates two accounts and updates one contact, and the contact update fails because of a validation rule failure, the entire transaction rolls back and none of the accounts are persisted in Salesforce
- Each DML statement accepts either a single sObject or a list (or array) of sObjects
  - Using a list of sObjects is considered a Bulk DML operation
  - Performing bulk DML operations is the recommended way because it helps avoid hitting governor limits
- The DML `merge` statement is a bit unique
  - The `merge` statement merges up to three records of the same sObject type into one of the records, deleting the others, and re-parenting any related records
- When inserting records, the system assigns an ID for each record
  - In addition to persisting the ID value in the database, the ID value is also autopopulated on the sObject variable that you used as an argument in the DML call
  - So this allows you to create a record using an sObject and then use that same sObject to perform additional DML operations, such as `updated`, as the system will be able to map the sObject variable to its corresponding record by matching the ID
- If you don’t specify a field when calling the `upsert` statement, it uses the sObject’s ID to match the sObject with existing records
  - For custom objects, specify a custom field marked as external ID
  - For standard objects, you can specify any field that has the idLookup property set to true
  - If the key is matched multiple times, an error is generated and the object record is neither inserted or updated
- You **cannot** retrieve a record from the database using DML
  - This needs to be done using SOQL
- Not seeing a clean way to see what the primary key is on an object
  - You can view an object in Salesforce and the primary key value will show up in the URL
  - But no easy way to look at a object in the Object Manager and see what the primary key is?
  - <https://developer.salesforce.com/forums/?id=9062I000000g4qUQAQ>
    - Seems to be the only way
    - Running this can show you that `Label: Account ID` & `API Name: Id` are idLookup fields but this field does not appear in the Object Manager
- Deleted records are placed in the Recycle Bin for 15 days from where they can be restored

#### Database Methods

- Apex contains the built-in Database class, which provides methods that perform DML operations and mirror the DML statement counterparts
- Unlike DML statements, Database methods have an optional `allOrNone` parameter that allows you to specify whether the operation should partially succeed
  - When this parameter is set to false, if errors occur on a partial set of records, the successful records will be committed and errors will be returned for the failed records
  - Also, no exceptions are thrown with the partial success option
  - By default, the allOrNone parameter is true
    - Which means that the Database method behaves like its DML statement counterpart and will throw an exception if a failure is encountered
- The Database methods return result objects containing success or failure information for each record
- The following 3 statements are identical because `allOrNone == true`
  - `insert recordList;`
  - `Database.insert(recordList);`
  - `Database.insert(recordList, true);`
- In addition to the methods that mirror DML, the Database class contains methods that aren’t provided as DML statements. For example
  - Methods used for transaction control and rollback
  - Emptying the Recycle Bin
  - Methods related to SOQL queries

### Should You Use DML Statements or Database Methods

- Use DML statements if
  - You want any error that occurs during bulk DML processing to be thrown as an Apex exception that immediately interrupts control flow (by using `try. . .catch` blocks). This behavior is similar to the way exceptions are handled in most database procedural languages
- Use Database class methods if
  - You want to allow partial success of a bulk DML operation—if a record fails, the remainder of the DML operation can still succeed. Your application can then inspect the rejected records and possibly retry the operation. When using this form, you can write code that never throws DML exception errors. Instead, your code can use the appropriate results array to judge success or failure
    - Note that Database methods also include a syntax that supports thrown exceptions, similar to DML statements

### SOQL Queries

- Salesforce Object Query Language (SOQL) is a language that gets record data from a Salesforce database
- They return an array of sObjects or a single sObject
- You **cannot** specify `*` for all fields
  - You must specify every field you want to get explicitly
  - If you try to access a field you haven’t specified in the SELECT clause, you’ll get an error because the field hasn’t been retrieved
  - **Except** you don’t need to specify the Id field in the query as it is always returned in Apex queries
- String comparisons on `WHERE` clauses are case sensitive
- Supports the typical syntax of: `LIKE '%some string%'`
- You can use SOQL for loops to avoid hitting the heap size limit
  - The results of a SOQL query can be iterated over within the loop
  - SOQL for loops use a different method for retrieving records, records are retrieved using efficient chunking with calls to the query and queryMore methods of the SOAP API
  - Example

    ```java
    for (variable : [soql_query]) {
      code_block
    }
    ```

    - In this example `variable` could be a single sObject or a list of sObjects(called the list format)
      - If variable is a single sObject the the loop will execute the same number of times as records returned by the query
        - Ex: If the query returns 500 records, it loops 500 times
      - If variable is a list of sObjects the loop will process 200 at a time
        - So if the query returns <= 200 results it will only execute once, if it returns > 200 it will iterate 2 or more times, processing 200 at a time
        - Ex: If the query returns 500 results it will loop 3 times
    - Example **using list format**

      ```java
      for (Account[] tmp : [SELECT Id FROM Account WHERE Name LIKE 'some text']) {
          //Do something
      }
      ```

### SOSL Queries

- Salesforce Object Search Language (SOSL) is a Salesforce search language that is used to perform text searches in records
- Use SOSL to search fields across multiple standard and custom object records
- Text searches are case-insensitive
- The SearchGroup in SOSL is optional
  - It is the scope of the fields to search
  - If not specified, the default search scope is all fields
  - SearchGroup can take one of the following values
    - ALL FIELDS
    - NAME FIELDS
      - This is kind of weird. Some fields seem to be categorized as Name Fields but some objects have more than one
      - See: [Local Name Fields](https://help.salesforce.com/articleView?id=admin_local_name_fields.htm)
    - EMAIL FIELDS
    - PHONE FIELDS
    - SIDEBAR FIELDS
- The SearchQuery in SOSL contains two types of text
  - Single word
    - Such as `test` or `hello`
    - Words in the SearchQuery are delimited by spaces, punctuation, and changes from letters to digits (and vice-versa)
    - Words are always case insensitive
  - Phrases
    - A collection of words and spaces surrounded by double quotes such as `"john smith"`
    - Multiple words can be combined together with **logic and grouping operators** to form a more complex query
- See Search Examples [here](https://trailhead.salesforce.com/content/learn/modules/apex_database/apex_database_sosl) for more examples
- The SOSL search results are returned in a list of lists. Each list contains an array of the returned records for each object requested
  - Unsure if a empty search result creates a empty array or if it is omitted
- You can filter, reorder, and limit the returned results of a SOSL query
  - Because SOSL queries can return multiple sObjects, those filters are applied within each sObject inside the `RETURNING` clause

### SOQL vs SOSL

- SOSL enables you to search text, email, and phone fields for multiple objects simultaneously
- SOQL only allows you to query one object at a time
- More/different DB indexes available in SOSL
- Syntax is different. SOQL is basically standard SQL where SOSL is a very different Salesforce search language
- SOSL matches fields based on a word match while SOQL performs an exact match by default (when not using wildcards)
  - For example, searching for 'Digital' in SOSL returns records whose field values are 'Digital' or 'The Digital Company', but SOQL returns only records with field values of 'Digital'
- The search query for SOSL in the Query Editor and the API must be enclosed within curly brackets (`{Wingo}`). In contrast, in Apex the search query is enclosed within single quotes (`'Wingo'`)
- SOQL and SOSL statements in Apex can reference Apex code variables and expressions if they are preceded by a colon (`:`)
  - The use of a local variable within a SOQL or SOSL statement is called a bind
  - Ex: `WHERE Department=:targetDepartment`
- [SOQL vs SOSL - Which one to use and when?](https://salesforce.stackexchange.com/questions/9028/soql-vs-sosl-which-one-to-use-and-when)
- [SOQL vs SOSL](https://developer.salesforce.com/forums?id=906F00000008y69IAA)
  - > SOSL can search multiple object types, which requires multiple separate queries in SOQL, in addition all the relevant fields are already text indexed for SOSL, but the same fields don't have DB indexes, so SOQL queries against them will be slower. If you have a lot of data, these differences will be much more apparent.
  - > In my development, it's 99% SOQL.  Where SOSL has come up is when there is a need to allow an end-user to take a "broad brush stroke" across the data to find a record to work further with.  Once found, SOQL queries take over.
    >
    > SOQL should be used when you need precision in what is returned.  With Salesforce functionality being so business process driven, precision is usually very important and that's why SOQL is used more often.  It can also be used to craft a search-like query, but it's probably meant more for precise queries.
    > SOSL can be used when precision is not as important and when you find yourself constructing a crazy WHERE clause in a SOQL query.  It might just be easier to use SOSL.  SOSL can give you a bit more assurance that records you want returned will be even if you end up with more data to sift through.  With SOQL, you are going field by field to match criteria and you might exclude records you don't want excluded.  Also, if you add new fields to the system, SOSL will pick up on those and search them whereas SOQL will not.
    > SOSL comes up in specific use cases for me, but most of the time SOQL works just fine too.

---

### Working with Related Records

- [Working with Related Records](https://trailhead.salesforce.com/content/learn/modules/apex_database/apex_database_dml#Tdxn4tBK-heading12)
- When inserting records related to existing records the relationship needs to already be configured
- Fields on related records can't be updated with the same call to the DML operation and require a separate DML call. For example
  - If inserting a new contact, you can specify the contact's related account record by setting the value of the AccountId field
  - However, you can't change the account's name without updating the account itself with a separate DML call
- The `delete` operation supports cascading deletions
  - If you delete a parent object, you delete its children automatically, as long as each child record can be deleted
- To get child records related to a parent record, add an inner query for the child records
  - Ex: `SELECT Name, (SELECT LastName FROM Contacts) FROM Account WHERE Name = 'SFDC Computing'`
    - `(SELECT LastName FROM Contacts)` is the subquery and `FROM Contacts` specifies the Contacts Child Relationship Name, which is a default **relationship** on Account that links accounts and contacts
      - The Child Relationship Name, Contacts, is defined on the Contacts object
      - This is a Lookup Relationship but same goes for Master-Detail Relationships
      - We know this is a standard relationship because it doesn't end with the `__r` suffix like custom relationships do
- You can traverse a relationship from a child object (contact) to a field on its parent (Account.Name) by using dot notation

---
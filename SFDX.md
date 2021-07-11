# SFDX

Salesforce Developer Experience CLI

[Salesforce CLI Command Reference](https://developer.salesforce.com/docs/atlas.en-us.sfdx_cli_reference.meta/sfdx_cli_reference/cli_reference.htm)

---

## Common Commands

Config

- `sfdx config:list` - Lists the config variables that the Salesforce CLI uses for various commands and tasks.
  - `sfdx config:get defaultusername`
  - `sfdx config:set defaultusername=Some-Alias-Here`

Auth

- `sfdx auth:list` - List auth connection information.
- `sfdx auth:web:login`
  - `sfdx auth:web:login -r https://somedomain--partialsb.my.salesforce.com`
  - `sfdx auth:web:login -a Some-Alias-Here -r https://somedomain--partialsb.my.salesforce.com`

Force:Schema

- `sfdx force:schema:sobject:describe -s Opportunity`

Force:mdapi

- `sfdx force:mdapi:describemetadata`

---

## TODO

### Example 1 - Diff Flow Versions

`sfdx auth:web:login -a Some-Alias-Here -r https://somedomain.my.salesforce.com`

`sfdx config:set defaultusername=Some-Alias-Here`

`sfdx force:mdapi:describemetadata`

Update `manifest/package.xml`

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<Package xmlns="http://soap.sforce.com/2006/04/metadata">
    <types>
        <members>File_Upload_Screen_Flow-14</members>
        <name>Flow</name>
    </types>
    <types>
        <members>File_Upload_Screen_Flow-15</members>
        <name>Flow</name>
    </types>
    <types>
        <members>File_Upload_Screen_Flow-16</members>
        <name>Flow</name>
    </types>
    <types>
        <members>File_Upload_Screen_Flow-17</members>
        <name>Flow</name>
    </types>
    <version>51.0</version>
</Package>
```

Retrieve meta data: `sfdx force:mdapi:retrieve --wait 10 --retrievetargetdir ./sfdx-output --unpackaged ./manifest/package.xml`

---

### Example 2

Lets say I want to compare two profiles. I can...

Check the meta data api name for "profiles": `sfdx force:mdapi:describemetadata`

Example output

```json
{
  "directoryName": "profiles",
  "inFolder": false,
  "metaFile": false,
  "suffix": "profile",
  "xmlName": "Profile"
}
```

Update `manifest/package.xml`

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<Package xmlns="http://soap.sforce.com/2006/04/metadata">
    <types>
        <members>*</members>
        <name>Profile</name>
    </types>
    <version>51.0</version>
</Package>
```

List all profiles: `sfdx force:mdapi:listmetadata -m Profile`

Optionally (now that you know the names of the profiles) update the `<members>` tag in `manifest/package.xml` to filter specific profiles

Retrieve meta data: `sfdx force:mdapi:retrieve --wait 10 --retrievetargetdir ./sfdx-output --unpackaged ./manifest/package.xml`

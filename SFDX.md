# SFDX

Salesforce Developer Experience CLI

[Salesforce CLI Command Reference](https://developer.salesforce.com/docs/atlas.en-us.sfdx_cli_reference.meta/sfdx_cli_reference/cli_reference.htm)

---

## Common Commands

List auth connection information

- `sfdx auth:list`

Authorize/re-authorize an org using the web login flow

- `sfdx auth:web:login`
- `sfdx auth:web:login -r https://somedomain--partialsb.my.salesforce.com`
- `sfdx auth:web:login -a Some-Alias-Here -r https://somedomain--partialsb.my.salesforce.com`

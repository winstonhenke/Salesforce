# Packaging and Distributing Apps

Docs about packing and distributing of managed and unmanaged packages

---

## General

- <https://help.salesforce.com/articleView?id=sf.package_distribute_apps_overview.htm>
- Org types involved
  - Developer Hub Org (Dev Hub)
    - Lets you create and manage Scratch Orgs
    - [Your Dev Hub org is often your production org](https://developer.salesforce.com/docs/atlas.en-us.sfdx_dev.meta/sfdx_dev/sfdx_dev_scratch_orgs_editions_and_allocations.htm) (See notes below about linking namespaces to a Dev Hub)
  - Packaging Org
    - A Developer Edition org configured with a managed package namespace.
  - Environment Hub Org
  - Scratch Org
    - [Can only be created from a Dev Hub](https://help.salesforce.com/articleView?id=sf.sfdx_setup_enable_devhub.htm)

[Understanding Packages](https://help.salesforce.com/articleView?id=sf.sharing_apps.htm)

- Unmanaged Packages
  - Typically used to distribute open-source projects or application templates to provide developers with the basic building blocks for an application.
  - Components **can** be edited in the organization they are installed in.
  - The developer who created and uploaded the unmanaged package has no control over the installed components, and can't change or upgrade them.
  - Unmanaged packages should **not** be used to migrate components from a sandbox to production organization. Instead, use **Change Sets**.
  - As a best practice, install an unmanaged package only if the org used to upload the package still exists. If that org is deleted, you may not be able to install the unmanaged package.
- Managed Packages
  - Two types: first-generation packaging (1GP) and second-generation packaging (2GP).
  - Typically used by Salesforce partners to distribute and sell applications to customers.
  - Must be created from a Developer Edition organization.
  - Using the AppExchange and the License Management Application (LMA), developers can sell and manage user-based licenses to the app.
  - Fully upgradeable.
  - Certain destructive changes, like removing objects or fields, can not be performed.
- All packages are unmanaged until you convert them to managed packages.

---

## Unmanaged Packages

- Not upgradeable. Would need to uninstall and reinstall.
- The package source code can be viewed by its recipients and modified freely, and it bears no remaining connections to the org where it originated. As such, the only way an unmanaged package can be modified, patched, or updated is if it’s edited directly, or if it’s uninstalled and replaced by a new copy of the package.
- Does not require a namespace.
  - [It *can* have a namespace but...](https://help.salesforce.com/articleView?id=sf.faq_distribution_installing_what_happens_to_my.htm&type=5)
    - >  Unmanaged packages can have a namespace prefix while they're developed in an org that contains a managed package. This namespace isn’t used outside of the development (publisher) org. If an unmanaged package is installed in an org that has no namespace, then the unmanaged components have no namespace in the subscriber org. If an unmanaged package is installed in an org that has a namespace, then the components get the namespace of the subscriber org.

To develop a unmanaged package the workflow would be...

- Initialize a Salesforce DX project locally
- Create a Scratch Org (requires a Dev Hub)
  - `sfdx force:org:create edition=Developer -s -a AliasForNewScratchOrg -v AliasForDevHubOrg`
- Push/pull changes from the Scratch Org
  - `sfdx force:source:push -u AliasForNewScratchOrg`
  - `sfdx force:source:pull -u AliasForNewScratchOrg`
- When ready convert from source format to metadata format and deploy to a Packaging Org
  - `sfdx force:source:convert --outputdir OutputFolderName --packagename SomeNameForThePackage`
  - `sfdx force:mdapi:deploy --deploydir SomeNameForThePackage -u AliasForPackagingOrg`
- Check the status of the deployment
  - `sfdx force:mdapi:deploy:report -u AliasForPackagingOrg`
- Upload the package in the Packaging Org
  - From the Packaging Org you deployed to open: `Setup -> Apps -> Packaging -> Package Manager`
  - Open the package you just deployed
  - Click `Upload`
  - Specify the new version information
- Once uploaded find the install URL
  - Open the versions tab on the new package from Package Manager in the Packaging Org
  - Select the version you want
  - From here you can see the Installation URL which can be used to install the package in another org
- **IMPORTANT** - Careful with temporary Development Orgs
  - If you delete an org that is hosting a unmanaged package that install link will no longer work

---

## Managed Packages

- The only requirement to create a managed package is that you’re using a Developer Edition organization.
- **TODO** - Figure out the details around the Partner Program, Environment Hub, Business Org, Partner Developer Edition Org, Etc
  - Discussed here in the [ISVforce Guide](https://resources.docs.salesforce.com/230/latest/en-us/sfdc/pdf/salesforce_packaging_guide.pdf)
- The Packaging Org is where new versions of a package are created. Every managed package has one and only one Packaging Org.

[Create and Register Your Namespace](https://developer.salesforce.com/docs/atlas.en-us.sfdx_dev.meta/sfdx_dev/sfdx_dev_dev2gp_create_namespace.htm)

- > With 2GP, you can share a single namespace with multiple packages. Since sharing of code is much easier if your package shares the same namespace, we recommend that you use a single namespace for all of your 2GP packages.

[Link a Namespace to a Dev Hub Org](https://help.salesforce.com/articleView?id=sf.sfdx_dev_reg_namespace.htm)

- > To use a namespace with a scratch org, you must link the Developer Edition org where the namespace is registered to a Dev Hub org.
- > If you don’t have an org with a registered namespace, create a Developer Edition org that is separate from the Dev Hub or Scratch Orgs.
  - So the name space is created and registered in a Development Org and then linked to the Dev Hub.
  - **TODO**
    - Salesforce says it's common for Prod to be the Dev Hub
    - A separate Dev Org needs to be created as the Packaging Org (this is where the namespace is created and registered)
    - So should this Packaging Org be a persisted Org? I.e not a disposable one that can be deleted after making a new version.
    - Is it common to use a single Packaging Org for multiple packages? Or is one-to-one more common?

---

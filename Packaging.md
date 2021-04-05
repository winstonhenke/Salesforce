# Packaging and Distributing Apps

Docs about packing and distributing of managed and unmanaged packages.

- <https://help.salesforce.com/articleView?id=sf.package_distribute_apps_overview.htm>
- <https://resources.docs.salesforce.com/230/latest/en-us/sfdc/pdf/salesforce_packaging_guide.pdf>

---

## General

Types of Orgs

- [Partner Business Org](https://partners.salesforce.com/s/education/general/Partner_Business_Org)
  - The Partner Business Org is the org you use to run your business, essentially the org where your CRM exists. Salesforce provides 2 free Enterprise Edition licenses to all contracted partners. For ISV Partners, core technologies (License Management App, Channel Order App) are utilized from this org.
  - This is needed if you are charging for your app on AppExchange. I believe you can still list free apps without this.
- [Environment Hub Org](https://help.salesforce.com/articleView?id=sf.environment_hub_intro.htm&type=5)
  - This is my Environment Hub Org: <https://phy69.my.salesforce.com>
    - **Note**: This is a Enterprise Edition Org. NOT a Dev Org.
  - Basically just an org that has enabled the Environment Hub. Mine was enabled by default since I created it when signing up for the Salesforce Partner Community. Alternatively you can submist a ticket to Salesforce to have this enabled on an existing org.
  - It lets you connect, create, view, and log in to Salesforce orgs from one location.
  - If your company has multiple environments for development, testing, and trials, the Environment Hub lets you streamline your approach to org management.
  - It is the only way that ISV partners can spin up a `Partner Developer Org`, which is used to build managed packages.
  - AppExchange Partners should never enable Environment Hub in the DE org that contains a managed package.
  - Once enabled, Environment Hub cannot be turned off.
  - As a best practice, one organization should be designated as the Environment Hub (or hub organization), which can then connect all your other organizations, or member organizations, to the hub.
  - You can establish single sign-on between the hub and member organizations, enabling users to seamlessly switch between them without having to provide login credentials.
  - From a Env Org you can create other types of Dev Orgs but...
    - [I'd strongly discourage package development in anything other than Partner Developer Edition orgs. (At least for packages intended for public consumption)](https://salesforce.stackexchange.com/a/53843/77466)
- Partner Developer Org
  - A developer org created from the Environment Hub
  - Very similar to a regular dev org but it's super sized allowing for a higher API call limit, more storage, and a greater number of licenses available for your team.
- Developer Hub Org (Dev Hub)
  - Lets you create and manage Scratch Orgs
  - See notes below about linking namespaces to a Dev Hub
  - TODO - Would I enable this on a Partner Developer Org?
- Packaging Org
  - A Developer Edition org configured with a managed package namespace.
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
    - My Packaging Org I'm using for testing says
      - > Your organization is configured to contain **one managed package** and an unlimited number of unmanaged packages. Only managed packages can be upgraded.
      - Is this because I'm using a free version? Do paid version allow more than one?

---

# Salesforce

Notes from learning about Salesforce

Questions

- How to best customize an app?
  - Ex. Say I have an app that uses a LWC that makes a callout and I want to configure the number of retries

---

## TOC

### README

- [Org Types](#org-types)
- [Notes to Self](#notes-to-self)
- [Definitions](#definitions)
- [Questions](#questions)
- [Initial Requirements Gathering](#initial-requirements-gathering)
- [Concepts](#concepts)
  - [Salesforce Platform](#salesforce-platform)
  - [Salesforce.com | Force.com](#salesforcecom---forcecom)
  - [Salesforce | Lightning Platform | Sales Cloud | Service Cloud | Etc](#salesforce---lightning-platform---sales-cloud---service-cloud---etc)
  - [Salesforce Apps](#salesforce-apps)
  - [Declarative Development](#declarative-development)
  - [Org](#org)
  - [Objects](#objects)
  - [Object Relationships](#object-relationships)
  - [Metadata](#metadata)
  - [Salesforce Classic vs Lightning Experience](#salesforce-classic-vs-lightning-experience)
  - [Lightning components, Apex, and Visualforce pages](#lightning-components--apex--and-visualforce-pages)
  - [Salesforce APIs](#salesforce-apis)
  - [Heroku](#heroku)
  - [Permissions](#permissions)
  - [Formulas & Validation](#formulas---validation)
  - [Navigation](#Navigation)

### Other Pages

- [Apex](/Apex.md)
- [Lightning](/Lightning.md)

---

## Org Types

- Production Org: The real deal
- Sandbox Org: A clone of a production Org
- Developer Edition Org(DE): A free Development Org that when first created is empty
- Trailhead Playground Org(TP): Basically a DE that has been pre configured for a Trailhead challenge

---

## Notes to Self

- To debug Javascript on a Lightning Component [enable debug mode for the user](https://developer.salesforce.com/docs/component-library/documentation/en/lwc/lwc.debug_mode_enable)
  - This will allow for more verbose logging and non minified JS
  - Will want to turn it off after since it does slow down your browsing

---

## Definitions

- SOQL: Salesforce Object Query Language
- SOSL: Salesforce Object Search Language
- LWC: Lightning Web Components
- SLDS: Salesforce Lightning Design System

---

## Questions

- When you clone a permission set it defaults the API name to be the same as the cloned permission set and doesn't force you to update it
  - How  can you  have two permission sets with the same API name? How do you distinguish between them and why doesn't this need to be unique
  - <https://trailhead.salesforce.com/content/learn/modules/data_security/data_security_objects?trail_id=force_com_dev_beginner&trailmix_creator_id=whenke&trailmix_slug=getting-started#Tdxn4tBK-heading11>

---

## Initial Requirements Gathering

Things to consider when taking on a project

- What platforms need to be supported
  - Salesforce Classic, Lightning Experience, Salesforce Mobile app
- Things like what currency locals need to be supported

---

## Concepts

### Salesforce Platform

- <https://trailhead.salesforce.com/content/learn/modules/starting_force_com/starting_understanding_arch?trailmix_slug=getting-started#Tdxn4tBK-heading2>
- Series of layers that sit on top of each other
  - At the bottom you have the standard and custom tables/objects along with your org's metadata
  - In the middle you have tools/mechanisms for manipulating, analyzing, exporting, integrating, etc. of that data
  - At the top you have the apps that use the middle layers and provide the GUI CRUD operations

---

### Salesforce.com | Force.com

- Basically Salesforce.com is the SaaS and Force.com is the PaaS
- Salesforce.com is the out the box solution and is built on top of Force.com
- Custom apps are built on the Force.com platform and can be used in your Salesforce.com products which are again hosted on Force.com
- Their marketing seems to call it the "Customer 360 Platform"?

---

### Salesforce | Lightning Platform | Sales Cloud | Service Cloud | Etc

- Salesforce is a platform that you can buy or build apps that run on the platform
- Salesforce has [prebuilt offerings/products](https://www.salesforce.com/products/) that you can buy from them. Two of the most popular ones are Sales and Service
  - [Sales Cloud vs Service Cloud](https://ledgeviewpartners.com/blog/salesforce-service-cloud-vs-sales-cloud/)
  - These products are also refereed to as Clouds or modules
  - [Types of Salesforce Clouds](https://medium.com/@cs.venkatesh95/types-of-salesforce-clouds-906a79750579)
    - **Sales**
      - Basically a module in salesforce where the entire sales process is handled and revenues are generated
    - **Service**
      - Provides services like call-center, live conversations, knowledgebase, assistance with products to customers. The main motive of the service cloud is to keep people happy.
    - **Marketing**
    - **Commerce**
      - Offers e-commerce solutions for B2C (business to consumer) and B2B (business to business) customers. It provides an e-commerce website to customers shopping online
    - **Community**:
- When you buy one of these products/modules/Clouds it basically just gives gives you access to a set of features and apps
  - Example, from what I can tell the main thing the Sales Cloud gets you is access to the Sales app from within your org. There is likely more though.
- Each Cloud is just geared towards solving a different set of problems
- Each of these different offerings are Salesforce apps that are built on top of the Lightning Platform
  - Theoretically with just the Lightning Platform you could build your own versions of Sales Cloud, Service Cloud, etc. from scratch - but for the most part you'd be better off buying the product license and customizing it from there.
- Some scenarios don't require purchasing any of the prebuilt Salesforce offerings like Sales Cloud or Service Cloud
  - Example if a company is just trying to automate and integrate some internal process that is very specific to their domain or workflow then a custom app built on the Lightning Platform might be all they need

---

### Salesforce Apps

- <https://stackoverflow.com/a/13993121>
  - An app in Salesforce is nothing but a container which contains in it - a name, a logo, and an ordered set of tabs.
  - All the metadata such as Objects, Visualforce Pages, Classes, etc are independent of an app. An app just helps to group things together visually. But internally the metadata has nothing to do with an app i.e. you can have the same tab, VF Page in multiple apps.

---

### Declarative Development

Salesforce really pumps the use of declarative development using their "metadata-driven architecture"

- Basically point and click > custom/imperative code
  - Hard to argue against not re-inventing the wheel as much as I hate point and click development
- They highly recommend using this by default whenever possible
- Resorting to custom/imperative code only when the platform doesn't offer you what you need

---

### Org

- Refers to a specific instance of Salesforce
- A company can have more than one org

---

### Objects

- What Salesforce calls tables in a database
- There are standard objects like Accounts and Contacts
- Also custom objects you can create when modeling things specific to your business
- There are several other types of objects
  - External objects
  - Platform events
  - BigObjects
- They can be classified as Enterprise Application objects or Light Application objects
- Records are rows in object database tables
- Fields are columns in object database tables
- You can still add custom fields to standard objects

---

### Object Relationships

- Two main types of object relationships
  - Lookup (basically a foreign key in SQL)
    - Typically, you use lookup relationships when objects are only related in some cases
    - Can be one-to-one or one-to-many
    - Objects in lookup relationships usually work as stand-alone objects and have their own tabs in the user interface
  - Master-Detail
    - In this type of relationship, one object is the master and another is the detail
      - The detail object doesn't work as a standalone object
    - The master object controls certain behaviors of the detail object, like who can view the detail’s data
    - With a master-detail relationship between Property and Offer, you can delete the property and all its associated offers from your system
    - When you’re creating master-detail relationships, you always create the relationship field on the detail object
- There is a third relationship type called a hierarchical relationship
  - The main difference is that hierarchical relationships are only available on the User object
  - You can use them for things like creating management chains between users

---

### Metadata

- The following metadata which makes up the structure of an org
  - Custom and standard objects/tables
  - Fields/columns
  - View definitions
  - Business processes
  - Configuration
  - Etc
- They like to tout their "metadata-driven architecture"

---

### Salesforce Classic vs Lightning Experience

- Salesforce Lightning Experience was their UI overhaul which launched in 2015 and replaced what is now called Salesforce Classic
- Lightning Experience was built with Lightning Components(see below)
- Salesforce announced in the spring 2019 release notes, that Salesforce will no longer be adding features to Classic

---

### Lightning components, Apex, and Visualforce pages

[Full Lightning docs here](/Lightning.md)

#### UI Rendering

- Lightning Component Framework
  - **Client side UI rendering**
  - A UI development framework similar to AngularJS or React for desktop and mobile
  - The Lightning Component framework is available in Lightning Experience & Salesforce Classic(using Visualforce pages)
  - You can build Lightning components using two programming models
    - Lightning Web Components model
    - The original Aura Components model
  - Lightning web components and Aura components can coexist and interoperate on a page
  - Lightning web components (LWC)
    - The newer component model
    - Custom HTML elements built using HTML and modern JavaScript
    - Lightning web components inherit global HTML attributes and events
    - LWC basically takes the form through native web standards that are in the browser. It means that no added abstraction layer like Aura Framework or any other framework, we only need standard JavaScript to develop.
  - Aura Components
    - The older component model, also known as lightning components
    - Relies less on open standards and more proprietary abstractions to do the same actions LWC can do with native JavaScript/HTML
- Visualforce
  - **Server side UI rendering**
  - Can run 'natively' in Salesforce Classic or in a iframe with some limitations in the Lightning Experience
  - Mobile is possible but much harder
  - A markup language that lets you create custom Salesforce pages with code that looks a lot like HTML, and optionally can use a powerful combination of Apex and JavaScript
  - Similar to technologies like: PHP, ASP, and JSP
  - In Lightning Experience, Visualforce runs inside an iframe that’s wrapped inside the larger Lightning Experience container
    - This iframe is constrained in some aspects. Example you will not have access to JavaScript global variables such as `window.location`. Things like the security context and CORS settings might be tricky to get right but the Lightning Experience container does expose APIs to help with this stuff.
    - See the `sforce.one` JavaScript utility object for example
  - Visualforce pages are just HTML pages with extra tags resolved by the server
    - As a result, you can use an empty Visualforce page as a container for a JavaScript application written in something like Angular or React
      - Sounds gross
  - Visualforce pages can be styled to look closer to the Lightning Experience using the Lightning Design System
    - Works decent for new apps and pages, can be challenging for older ones
- Lightning components | Visualforce pages
  - [User Interface Development Considerations](https://trailhead.salesforce.com/content/learn/modules/lex_dev_overview/lex_dev_overview_future)
  - An app would use either Lightning components or Visualforce pages, not both
  - Lightning components: app-centric, Visualforce pages: page-centric. Think traditional [web app vs SPA](https://trailhead.salesforce.com/content/learn/modules/lex_dev_overview/lex_dev_overview_future?trail_id=lex_dev&trailmix_slug=getting-started)
    - Visualforce pages are just HTML pages with extra tags resolved by the server
    - It's possible to have a empty Visualforce page acting as a container for a reactive Javascript application written in something like Angular
  - Both can use Apex controllers though
  - With Lightning components, you’re developing components that can be pieced together to create pages. With Visualforce, you’re developing entire pages at once
  - While you can develop mobile apps with Visualforce, none of the built-in components are mobile-savvy. Which means you write more code. Lightning Components, on the other hand, is specifically optimized to perform well on mobile devices
  - Lightning components are the new shiny tool but not always better. Visualforce is still very relevant due to how long its been around and has scenarios where it makes sense over a Lightning component

#### Server Side Business Logic

- Apex
  - **Server side business logic**
  - Salesforce's proprietary programming language with Java-like syntax
  - Strongly typed, object-oriented programming language that allows developers to execute flow and transaction control statements on Salesforce servers in conjunction with calls to the API
  - Apex code can be initiated by Web service requests and from triggers on objects

---

### Salesforce APIs

- Lightning platform APIs
  - REST
  - Soap
  - Bulk
  - Streaming

---

### Heroku

- Salesforce has tight integration with Heroku
  - Salesforce bought Heroku in 2010
- Heroku is a platform focused on the processes of deploying, configuring, scaling, tuning, and managing apps as simple and straightforward as possible
- Heroku Connect unifies your Salesforce data with your Heroku Postgres data so you don’t have to manage moving information across platforms
- So you can write a app, deploy/host it in Heroku, and have tight integration with Salesforce
- You can have a combination of Visualforce or Lightning components, microservices hosted on Heroku

---

### Permissions

- [Overview of Data Security](https://trailhead.salesforce.com/content/learn/modules/data_security)
- Profiles should contain the bare minimum permissions for a role. Then use permission sets to extend permissions for specific groups of users
- Permissions can be set per: org | object | field | record
- Permissions of an object on the detail side of a master-detail relationship automatically inherits the sharing setting of its parent. You can not set permissions on the detail object

#### Record Level Permissions

- [Control Access to Records](https://trailhead.salesforce.com/content/learn/modules/data_security/data_security_records)
- You can allow particular users to view an object, but then restrict the individual object records they're allowed to see
- The permissions on a record are always evaluated according to a combination of object-level, field-level, and record-level permissions
- When object-level permissions conflict with record-level permissions, the most restrictive settings win
- You can manage record-level access in these four ways(listed in order of increasing access)
  - Organization-wide (defaults)
    - Specify the default level of access users have to each others' records
    - You use org-wide sharing settings to lock down your data to the most restrictive level, and then use the other record-level security and sharing tools to selectively give access to other users
  - Role hierarchies
    - Give access for users higher in the hierarchy to all records owned by users below them in the hierarchy
    - Only used to give additional users access to records. They can’t be stricter than your organization-wide default settings
  - Sharing rules
    - Automatic exceptions to organization-wide defaults for particular groups of users, so they can get to records they don’t own or can’t normally see
    - Sharing rules, like role hierarchies, are only used to give additional users access to records. They can’t be stricter than your organization-wide default settings
  - Manual sharing
    - Allows owners of particular records to share them with other users. Although manual sharing isn’t automated like org-wide sharing settings, role hierarchies, or sharing rules, it can be useful in some situations, such as when a recruiter going on vacation needs to temporarily assign ownership of a job application to someone else
- The sharing model for an object can be set to one of these settings
  - Private
    - Only the record owner, and users above that role in the hierarchy, can view, edit, and report on those records
  - Public Read Only
    - All users can view and report on records, but only the owner, and users above that role in the hierarchy, can edit them
  - Public Read/Write
    - All users can view, edit, and report on all records
  - Controlled by Parent
    - A user can view, edit, or delete a record if she can perform that same action on the record it belongs to

---

### Formulas & Validation

- Formula fields calculate values using fields within a single record
- Roll-up summary fields calculate values from a set of related records
  - You can create roll-up summary fields that automatically display a value on a master record based on the values of records in a detail record
  - These detail records must be directly related to the master through a **master-detail relationship**
  - You define a roll-up summary field on the object that is on the master side of a master-detail relationship

---

### Navigation

Salesforce uses the term navigation to be a little more broad than I was expecting

- > What do we actually mean by “navigation”? The first thing we might mean by navigation is user interface elements on the screen. You click something, and something happens. For example, you click the Accounts item in the navigation menu, and you go to the Accounts object home page. You click the New button, and a record entry form appears. You choose a custom action from a quick actions menu, and you launch a custom process. And so on. Those buttons and menu items are navigation elements.
  - It's the “something happens” part that I wouldn't have considered navigation

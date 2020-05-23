# Salesforce

Notes from learning about Salesforce

---

## Definitions

- SOQL: Salesforce Object Query Language
- LWC: Lightning Web Components
- SLDS: Salesforce Lightning Design System

---

## Concepts

### Salesforce Platform

- <https://trailhead.salesforce.com/content/learn/modules/starting_force_com/starting_understanding_arch?trailmix_slug=getting-started#Tdxn4tBK-heading2>
- Series of layers that sit on top of each other

---

### Salesforce | Lightning Platform | Sales Cloud | Service Cloud | Etc

- Salesforce is a platform that you can buy or build apps that run on the platform
- Salesforce has [prebuilt offerings/products](https://www.salesforce.com/products/) that you can buy from them. Two of the most popular ones are Sales and Service
  - These products are also refereed to as Clouds or modules
  - Example Sales Cloud and Service Cloud
- When you buy one of these products/modules/Clouds it basically just gives gives you access to a set of features and apps
  - Example, from what I can tell the main thing the Sales Cloud gets you is access to the Sales app from within your org. There is likely more though.
- Each Cloud is just geared towards solving a different set of problems
- Each of these different offerings are Salesforce apps that are built on top of the Lightning Platform
  - Theoretically with just the Lightning Platform you could build your own versions of Sales Cloud, Service Cloud, etc. from scratch - but for the most part you'd be better off buying the product license and customizing it from there.
- Some scenarios don't require purchasing any of the prebuilt Salesforce offerings like Sales Cloud or Service Cloud
  - Example if a company is just trying to automate and integrate some internal process that is very specific to their domain or workflow then a custom app built on the Lightning Platform might be all they need

---

### Declarative Development

Salesforce really pumps the use of "declarative development" using their "metadata-driven architecture"

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
- There a third relationship type called a hierarchical relationship
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

#### UI Rendering

- Lightning Component Framework
  - **Client side UI rendering**
  - A UI development framework similar to AngularJS or React for desktop and mobile
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
  - Mobile is possible but much harder
  - A markup language that lets you create custom Salesforce pages with code that looks a lot like HTML, and optionally can use a powerful combination of Apex and JavaScript
  - Similar to technologies like: PHP, ASP, and JSP
  - Visualforce pages are just HTML pages with extra tags resolved by the server
    - As a result, you can use an empty Visualforce page as a container for a JavaScript application written in something like Angular or React
      - Sounds gross
- Lightning components | Visualforce pages
  - [User Interface Development Considerations](https://trailhead.salesforce.com/content/learn/modules/lex_dev_overview/lex_dev_overview_future)
  - An app would use either Lightning components or Visualforce pages, not both
  - Lightning components: app-centric
  - Visualforce pages: page-centric
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

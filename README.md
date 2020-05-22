# Salesforce

Notes from learning about Salesforce

---

## Definitions & Concepts

### Salesforce Platform

- <https://trailhead.salesforce.com/content/learn/modules/starting_force_com/starting_understanding_arch?trailmix_slug=getting-started#Tdxn4tBK-heading2>
- Series of layers that sit on top of each other

---

### Salesforce vs Lightning Platform vs Sales Cloud vs Service Cloud vs Marketing Cloud vs Community Cloud vs Etc

- Salesforce is a platform that you can buy or build apps that run on the platform
- Salesforce has [prebuilt offerings/products](https://www.salesforce.com/products/) that you can buy from them. Two of the most popular ones are Sales and Service
  - These products are also refereed to as Clouds or modules
  - Example Sales Cloud and Service Cloud
- When you buy one of these products/modules/Clouds it basically just gives gives you access to a set of features and apps
  - Example, from what I can tell the main thing the Sales Cloud gets you is access to the Sales app from within your org. There is likely more though.
- Each Cloud is just geared towards solving a different set of problems
- Each of these different offerings are Salesforce apps that are built on top of the Lightning Platform
  - Theoretically with just the Lightning Platform you could build your own versions of Sales Cloud, Service Cloud, etc. from scratch - but for the most part you’d be better off buying the product license and customizing it from there.
- Some scenarios don't require purchasing any of the prebuilt Salesforce offerings like Sales Cloud or Service Cloud
  - Example if a company is just trying to automate and integrate some internal process that is very specific to their domain or workflow then a custom app built on the Lightning Platform might be all they need

---

### Org

- Refers to a specific instance of Salesforce
- A company can have more than one org

---

### Objects

- What Salesforce calls tables in a database
- There are standard objects like Accounts and Contacts and custom objects you can create when modeling things specific to your business
- Records are rows in object database tables
- Fields are columns in object database tables
- You can still add custom fields to standard objects

---

### Lightning components, Apex code, and Visualforce pages

- TODO

---

### Salesforce Classic and Lightning Experience

- Lightning experience is the new UI framework used to display information in Salesforce
- TODO

---

# Deltek

Notes from looking at the Deltex/Salesforce integration

Enterprise resource planning (ERP)

- Refers to a type of software that organizations use to manage day-to-day business activities such as accounting, procurement, project management, risk management and compliance, and supply chain operations. A complete ERP suite also includes enterprise performance management, software that helps plan, budget, predict, and report on an organization’s financial results.

---

## Deltek | Ajera

Deltek: All about streamlining business workflows for companies in a variety of industries

Deltek Ajera: Solution tailored for Architecture & Engineering(A&E) projects

- <https://www.deltek.com/en/products/project-erp/ajera>
- Capabilities
  - Project Accounting
  - Project Management
  - Business Intelligence

---

## Questions

On the surface it looks pretty straight forward. Most of my questions are around how to test the install and initial data migration for the app  in a way where I wouldn't mess up their systems. In the development world I'm used to we have various environments where we can test changes before they get to production and these environments are completely isolated but mimic the upper environments. Salesforce supports this but if you have a playground/sandbox org that's connected to something like Ajera and it isn't a test environment you need to be extremely careful. If this was a one way integration into salesforce the concern is much lower since then I could use a sandbox.

- I'm thinking this is something that is looked at on a case by case basis?
- Ajera does not look to be a SaSS solution so it would be up to the company you are working with to provide a testing Ajera API environment for testing purposes
  - Most companies that are familiar with working on system integration projects will already have this but companies that aren't might not
  - If they don't offer this I'd have more questions about how to do this safely
- I'm not sure if my development experience is making me overly cautious here but doing something like this on a live system without ever testing in a lower environment would be big no no normally
  - Is it common for a project like this to perform a test run?
  - Is this project a cookie cutter project where these concerns are overkill?
- I'm curious how the Salesfore app authenticates with the Ajera API but I have a feeling this is is handled during the install and likely not a big deal

If I was going to start this knowing what I know now, what I would do at a high level is

- Create a sandbox Salesforce org. Would be ideal if it was created as a copy of the production org of the company you are working with
- Ask them for credentials to their Ajera test environment(assuming they have one)
- Probably email support for that app asking if they have any more technical docs on it since everything I can find looks very sales'y/marketing'y
- Install the app in the sandbox
- Test the integrations in both directions. Deal with any issues
- Find any custom objects/fields/triggers/relationships that might impact or be impacted by the integration and deal with those
  - I'm thinking custom objects/fields your partner might have that might conflict with assumptions the app made
  - And also custom objects/fields added by the app that might have conflicts
- Document any nuances with the process
- Normally in my world your partner would also want to test it out/see it working. Not sure what that interaction normally looks like
- Install or promote the app to their production org and make any changes needed for issues discovered in the testing done in the bullet points above

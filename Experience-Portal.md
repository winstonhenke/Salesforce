# Experience Portal

Experience Cloud Notes

Resources

- [Customer Portal](http://resources.docs.salesforce.com/latest/latest/en-us/sfdc/pdf/customer_portal_guide.pdf)
- [Trailhead](https://trailhead.salesforce.com/en/content/learn/projects/communities_share_crm_data)
- [Considerations for Deploying Experience Cloud Sites with Change Sets](https://developer.salesforce.com/docs/atlas.en-us.communities_dev.meta/communities_dev/networks_migrate_considerations.htm#unsupported)

---

## General

- A Salesforce user must belong to a Role before they can enable a Contact as a portal user.
- Salesforce recommends cloneing the default profiles that are created after enabling Experience Cloud.
- Portal Users can not view Reports that live in the Public folder. Had to create a new folder with the appropriate sharing setup.
- For notifications to work I had to [open a support ticket with Salesforce](https://help.salesforce.com/articleView?id=000314990&type=1&mode=1) to enable them.

---

## Expressions - Typically used for record ID

- [Expressions Available for Displaying Current User Information](https://help.salesforce.com/articleView?id=sf.siteforce_communities_data_code_reference.htm&type=5)
- `{!CurrentUser.accountId}`
- `{!recordId}`

Should work but doesn't

```text
-- When using the Related Record List, assuming a 'Asset Tactics' is a related list on Accounts
-- and the component is being added to a Case detail page which has a master/detail with Accounts
-- This would work (hard coding the Parent Record ID)
Parent Record ID: 0015e000006h1sHAAQ
Related List Name: Asset_Tactics__r

-- But no way to dynamically get the Case's partent(an Account) ID
{!recordId.Account.Id}
{!recordId.Account}
{!recordId.AccountID}

```

Limitation: [Pass a value other than {!recordId} to a Lightning Component in a Community](https://trailblazer.salesforce.com/ideaView?id=0873A0000003bCSQAY)

---

## Welcome Emails & Login Issues

- If modifying the login page set `Login Page Type` to `Default Page` in Workspaces -> Login & Registration
- Welcome emails will not be sent until the site is activated. By default after creation it is not active.
  - If you already created a user the welcome email will be sent once the site is activated.
- If you try logging in after activating a new site but are redirected right back to the login screen…
  - You probably didn’t publish the portal. Until you do this there is nothing to view.
- WARNING: Once you mark a site as active, all members will get the welcome email. (Best to check what users have the portal account role)
  - Likely only a concern if the partner had tried setting up a different portal site in the past
  - I’m not sure what happens if a site is active already and then you add a Role… Do all those users then get an email?

---

## Hot Spots

- `Setup -> Security -> Sharing Settings`
- `Setup -> Portal Health Check`
- `Setup -> Feature Settings -> Digital Experiences -> Settings -> Sharing Sets`
- `Setup -> Feature Settings -> Digital Experiences -> All Sites -> Workspaces -> Administration -> Login & Registration`
  - Login Page Setup: I had a new site default to `Experience Builder Page` which is not controlled by the branding options above it. Set this to `Default Page` for these to take affect.

---

## Permissions

- If you have a custom object that is on the detail side of a master/detail with a standard object like Account, and you want the portal user to have the ability to create child records...
  - Allow Read access to the parent object.
  - Update minimum access level required on the Master-Detail Options in the child object. Set to Read Only.

---

## Branding

- A background image will not show on the login page until your enable: `Let guest users view asset files and CMS content available to the site` from Workspaces -> Preferences

### Always Inspect the DOM for Exact CSS Selectors

When setting the max page width for a site the navigation bar kept a large margin. To fix it I had to use edit the CSS on the theme with the following.

```css
.forceCommunityGlobalNavigation community_navigation-global-navigation-list {
  margin-left: 0;
}
```

Example CSS overrides done for a Customer Portal

```css
/* Navigation Bar */
.forceCommunityGlobalNavigation community_navigation-global-navigation-list {
  margin-left: 0;
}

/* Header Background - White */
.siteforceServiceBody .cHeaderOverlay {
  background: rgb(255, 255, 255) !important;
}

/* Search Box Border- Pinnacle Grey */
.siteforceServiceBody .cSearchPublisher .forceCommunityGlobalSearchInput .forceSearchInputDesktop .contentWrapper {
  border-color: rgb(100, 101, 103) !important;
}

/* Search Box Text- Pinnacle Grey */
.siteforceServiceBody .cSearchPublisher .forceCommunityGlobalSearchInput .forceSearchInputDesktop input, .siteforceServiceBody .cSearchPublisher .forceCommunityGlobalSearchInput .forceSearchInputDesktop input::placeholder, .siteforceServiceBody .cSearchPublisher .forceCommunityGlobalSearchInput .forceSearchInputDesktop .contentWrapper .slds-input__icon {
  color: rgb(100, 101, 103) !important;
}

/* User Profile Menu - Pinnacle Grey */
.siteforceServiceBody .cProfileCon a.trigger-link {
  color: rgb(100, 101, 103) !important;
  text-shadow: none !important;
}

/* Page Header - No Max Width */
.siteforceServiceBody .cHeaderLine {
  max-width: none !important;
}

```

---

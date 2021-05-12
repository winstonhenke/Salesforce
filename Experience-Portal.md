# Experience Portal

Experience Cloud Notes

Customer Portal: <http://resources.docs.salesforce.com/latest/latest/en-us/sfdc/pdf/customer_portal_guide.pdf> 
Trailhead: <https://trailhead.salesforce.com/en/content/learn/projects/communities_share_crm_data>

---

## Expressions - Typically used for record ID

- [Expressions Available for Displaying Current User Information](https://help.salesforce.com/articleView?id=sf.siteforce_communities_data_code_reference.htm&type=5)
- `{!CurrentUser.accountId}`
- `{!recordId}`

---

## Welcome Emails & Login Issues

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

---

## Branding

When setting the max page width for a site the navigation bar kept a large margin. To fix it I had to use edit the CSS on the theme with the following.

```css
.forceCommunityGlobalNavigation community_navigation-global-navigation-list {
  margin-left: 0;
}
```

---

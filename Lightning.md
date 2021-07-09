# Lightning Components

Lightning Web Components and Aura

- [Lightning Components](#lightning-components)
  - [General](#general)
  - [Aura Components](#aura-components)
    - [Design Considerations](#design-considerations)
      - [Delegate or not to delagate?](#delegate-or-not-to-delagate)
    - [Example Application](#example-application)
  - [Lightning Web Components](#lightning-web-components)

---

## General

Lightning Components: Client side UI rendering

- Primarily used in Lightning Experience but can also be used in Salesforce Classic [using Visualforce pages](https://developer.salesforce.com/docs/atlas.en-us.lightning.meta/lightning/components_visualforce.htm)
- You can build Lightning components using two programming models
  - Lightning Web Components model (LWC)
  - The original Aura Components model
- Lightning Web Components (LWC)
  - The newer component model
  - Custom HTML elements built using HTML and modern JavaScript
  - Lightning web components inherit global HTML attributes and events
  - LWC basically takes the form through native web standards that are in the browser. It means that no added abstraction layer like Aura Framework or any other framework, we only need standard JavaScript to develop.
- Aura Components
  - The older component model, also known as Lightning Components
  - Relies less on open standards and more proprietary abstractions to do the same actions LWC can do with native JavaScript/HTML
- A UI development framework similar to Angular or React for desktop and mobile
  - It is also possible to write Lightning Platform apps using frameworks such as Angular or React by embedding them in a Visualforce page [using what they call a container page](https://developer.salesforce.com/docs/atlas.en-us.224.0.pages.meta/pages/pages_html_container_page.htm). But it can be a bit cumbersome.
- Lightning Web Components and Aura components can coexist and interoperate on a page
- Mobile first design
- Lightning Page/Application vs Lightning Component
  - A Lightning page/application is a custom layout that lets you design pages for use in the Salesforce mobile app or in Lightning Experience. You can use a Lightning Page to create an app home page and add your favorite Lightning component
- Where can you use Lightning Components
  - Create stand-alone apps that are hosted on Salesforce
  - Custom tabs in another app
  - Create apps that are hosted on other platforms with Lightning Out, including embedding them into apps from those platforms
  - Reuse custom Lightning components in the "Lightning App Builder and Experience Builder"
  - Customize Lightning Experience record pages by adding a Lightning component
  - Create actions using a Lightning component, and then add the action to an object’s page layout to make it instantly accessible from a record page
  - Override an action with a Lightning component
  - Run Lightning components apps inside Visualforce pages
  - Customize flow screens
- Consider using a container app for developing/testing Lightning components
- Apex and Salesforce in general are **case-insensitive**, but JavaScript is case-sensitive. That is, `Name` and `name` are the same in Apex, but different in JavaScript.
- [Lightning Web Components reference](https://developer.salesforce.com/docs/component-library/overview/components)

---

## Aura Components

- [Aura Components Basics](https://trailhead.salesforce.com/content/learn/modules/lex_dev_lc_basics?trail_id=lex_dev&trailmix_slug=getting-started)
- In the Aura component programming model, a **component** is a bundle of code
  - Aura components are made up of Javascript controllers, stylesheets, XML markup that uses both static HTML tags with custom Aura component tags, and more
  - Related resources are “auto-wired” to each other, and together they make up the component bundle
  - A bundle is sort of like a folder. It groups the related resources for a single component. Resources in a bundle are auto-wired together via a naming scheme for each resource type. Auto-wiring just means that a component definition can reference its controller, helper, etc., and those resources can reference the component definition. They are hooked up to each other (mostly) automatically.
- Create Aura components/lightning bundle in the developer console, [example](https://trailhead.salesforce.com/content/learn/modules/lex_dev_lc_basics/lex_dev_lc_basics_create?trail_id=lex_dev&trailmix_slug=getting-started)
  - When viewing the bundle in the dev console notice the extra controls that appear in a palette on the right side of the editing window of any Lightning bundle. Each of the different buttons with a Create label represents a different resource you can add to the bundle
- When you create a...
  - Lightning component you will get a `.cmp` file with a root `<aura:component></aura:component>` tag for a Lightning component
  - Lightning application you will get a `.app` file with a root `<aura:application ></aura:application>` tag for a Lightning app
- An app is a special kind of component
  - When writing markup, you can add a component to an app, but you can’t add an app to another app, or an app to a component
  - An app has a standalone URL that you can access while testing, and which you can publish to your users
  - You can’t add apps to Lightning Experience or the Salesforce app, you can only add components. After the last unit this might sound weird; what exactly do you add to the App Launcher, if not an app? What you add to App Launcher is a Salesforce app, which wraps up an Aura component, something defined in a `<aura:component>`. An Aura components app, that is, something defined in a `<aura:application>`, can’t be used to create Salesforce apps. A bit weird, but there it is.
  - You publish functionality built with Lightning Components in containers. Lightning Component apps are one kind of container for our Lightning components.
  - This usually means that you build all of your “app” functionality inside a top-level component. Then at the end, you stick that one component in a container—maybe a Lightning Components app, maybe the Salesforce app, maybe something else.
- Despite the technical differences between a component and an app, when they talk about "creating apps" they really mean building functionality inside a component bundle, not an application bundle, because that’s how you build “apps” in the real world.
- Aura components can accept input in the form of attributes when they are created
  - An attribute is defined using an `<aura:attribute>` tag, which requires values for the name and type attributes, and accepts these optional attributes: default, description, required.
  - See [Attribute Data Types here](https://trailhead.salesforce.com/content/learn/modules/lex_dev_lc_basics/lex_dev_lc_basics_attributes_expressions?trail_id=lex_dev&trailmix_slug=getting-started)
  - See example below
- Component Expressions
  - An expression is any set of literal values, variables, sub-expressions, or operators that can be resolved to a **single value**.
    - Sub-expressions basically means you can use parenthesis to group things together
  - You can **not** use JavaScript in expressions in Aura components markup
  - You **can** pass them to another component to set the value on that component.
  - A formula, or a calculation, which you place within expression delimiters `{!` and `}`
    - `{!<expression>}`
    - `{!'Hello! ' + v.message}`
    - `{!$Label.c.Greeting + v.message}`
    - `{!v.account.Id}`
    - `{#v.attrib}`
  - What’s the `v.` part?
    - `v` is something called a value provider.
      - Value providers are a way to group, encapsulate, and access related data.
      - Value providers are a complicated topic, so for now, think of `v` as an automatic variable that’s made available for you to use.
      - `v.` gives you a hook to access the component’s message attribute, and it’s how you access all of a component’s attributes.
    - Types of Value Providers
      - `v.` - View: how you access all of a component’s attributes.
      - `a.` - Action
      - `c.` - Controller: component’s client/server-side controller
        - Context matters here. `c.` from the **component** markup references the client-side controller. `c.` from the controller code references the server-side controller.
        - And then `c:` (notice colon not dot) in the markup is the default namespace
    - More on these Value Providers
      - [When to use {#v.attrib} vs {!v.attrib}?](https://salesforce.stackexchange.com/questions/138348/when-to-use-v-attrib-vs-v-attrib)
      - [Value Providers - Salesforce Dev Docs](https://developer.salesforce.com/docs/atlas.en-us.lightning.meta/lightning/expr_source.htm)
      - [What are "v" and "c" in the Expenses example in the Lightning Component Developers Guide?](https://developer.salesforce.com/forums/?id=906F0000000AoVeIAK)
      - [Deeper understanding of Lightning Components and/or JavaScript](https://developer.salesforce.com/forums?id=906F0000000AndMIAS)
- Attributes on components are like instance variables in objects
- Attribute Data Types
  - Primitives
  - Standard and custom Salesforce objects
  - Collections, such as List, Map, and Set
  - Custom Apex classes
  - Framework-specific types, such as Aura.Component, or Aura.Component[]
- Lightning Components and MVC
  - There are similarities, to be sure, but it would be more correct to say that Lightning Components is View-Controller-Controller-Model, or perhaps View-Controller-Controller-Database.
  - Why is “controller” doubled up in that pattern name? Because when interacting with Salesforce, your components will have a server-side controller in addition to the client-side controller we’ve worked with in this unit. This dual controller design is the key difference between Lightning Components and MVC.
- **TODO**
  - `event.getSource()` vs `component.find()` in a component's controller
- **TODO**
  - Where can Aura apps/components be used. Standalone URL apps, app launcher, Lightning App Builder?
  - Where can LWCs be used?
- `$A`
  - A framework global variable that provides a number of important functions and services. Example: `$A.enqueueAction(action)`, adds the server call to the Aura component framework request queue.
  - [$A namespace](https://developer.salesforce.com/docs/atlas.en-us.lightning.meta/lightning/ref_jsapi_dollarA.htm)
- Action callback format
  - `action.setCallback(scope, callbackFunction);`
  - `action.setCallback(this, function(response) { ... });`
  - Callback functions take a single parameter, response, which is an opaque object that provides the returned data, if any, and various details about the status of the request.

---

### Design Considerations

#### Delegate or not to delagate?

Say you have a Aura app with this structure

```text
- Expenses.cmp
  - ExpenseList.cmp
    - ExpenseItem.cmp
```

```html
<!-- Expenses.cmp -->
<aura:component controller="ExpensesController">
  <!-- Component's Input -->
  <aura:attribute name="expenses" type="Expense__c[]" />
  <!-- ... -->
  <!-- Expense List component-->
  <c:expensesList expenses="{!v.expenses}" />
</aura:component>
```

```html
<!-- ExpenseList.cmp -->
<aura:component>
  <!-- Component's Input -->
  <aura:attribute name="expenses" type="Expense__c[]" />
  <aura:iteration items="{!v.expenses}" var="expense">
    <!-- Use the expenseItem component -->
    <c:expenseItem expense="{!expense}" />
  </aura:iteration>
</aura:component>
```

```html
<!-- ExpenseItem.cmp -->
<aura:component>
  <aura:attribute name="expense" type="Expense__c" />
  <!-- ... -->
  <lightning:input
    type="toggle"
    label="Reimbursed?"
    name="reimbursed"
    class="slds-p-around_small"
    checked="{!v.expense.Reimbursed__c}"
    messageToggleActive="Yes"
    messageToggleInactive="No"
    onchange="???"
  />
</aura:component>

```

And notice the `onchange="???"` in ExpenseItem.cmp, should `ExpenseItem.cmp` handle calling a server side function? Or should it send an event to it's grandparent `Expenses.cmp`? There are trade-offs either way but by having all child components delegate responsibility for handling server requests and for managing the expenses array attribute, we’re breaking encapsulation a bit.

---

### Example Application

- `harnessApp.app`: App definition resource
  - No matter how much functionality we decide we’re going to add to our helloWorld “app”, it’s all going to go inside the helloWorld component
  - This is our "container" or "harness" app

```xml
<!-- extends="force:slds" gives you all the Lightning Design System styles and design tokens-->
<aura:application extends="force:slds">
  <!--The harnessApp contains the helloWorld component-->
  <c:helloWorld/>
</aura:application>
```

- `helloWorld.cmp`: Component definition resource

  ```xml
  <aura:component >
    <p>Hello Lightning!</p>
  </aura:component>
  ```

Example Component with attributes

```xml
<aura:component>
  <aura:attribute name="message" type="String"/>
  <!--v is a value provider for the view, which is the helloMessage component itself-->
  <p>Hello! {!v.message}</p>
</aura:component>
```

---

## Lightning Web Components

todo

---

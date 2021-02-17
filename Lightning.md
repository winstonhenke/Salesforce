# Lightning Components

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
- [Lightning Web Components reference](https://developer.salesforce.com/docs/component-library/overview/components)

---

## TOC

- [Aura Components](#aura-components)
- [Lightning Web Components](#lightning-web-components)

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
  - You can’t add apps to Lightning Experience or the Salesforce app—you can only add components. After the last unit this might sound weird; what exactly do you add to the App Launcher, if not an app? What you add to App Launcher is a Salesforce app, which wraps up an Aura component, something defined in a `<aura:component>`. An Aura components app—that is, something defined in a `<aura:application>` —can’t be used to create Salesforce apps. A bit weird, but there it is.
  - You publish functionality built with Lightning Components in containers. Lightning Components apps are one kind of container for our Lightning components.
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
  - What’s the `v.` part?
    - `v` is something called a value provider. Value providers are a way to group, encapsulate, and access related data. Value providers are a complicated topic, so for now, think of `v` as an automatic variable that’s made available for you to use.
    - `v` gives you a hook to access the component’s message attribute, and it’s how you access all of a component’s attributes.
    - `v.` = View, `a.` = Action, `c.` = Controller
    - More on these Value Providers
      - [What are "v" and "c" in the Expenses example in the Lightning Component Developers Guide?](https://developer.salesforce.com/forums/?id=906F0000000AoVeIAK)
      - [Deeper understanding of Lightning Components and/or JavaScript](https://developer.salesforce.com/forums?id=906F0000000AndMIAS)

Example Application

- `harnessApp.app`: App definition resource
  - No matter how much functionality we decide we’re going to add to our helloWorld “app”, it’s all going to go inside the helloWorld component
  - This is our "container" or "harness" app

```xml
<aura:application >
  <!--The harnessApp contains the helloWorld component-->
  <c:helloWorld/>
</aura:application>
```

Example Component

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

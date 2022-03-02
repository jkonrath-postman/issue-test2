---
title: "Using variables"
order: 24
page_id: "variables"
updated: 2021-12-20
search_keyword: "collectionVariables, iterationData, collectionVariables.set, collectionVariables.get, JSON.stringify, JSON.parse, base_url, pm.globals.set, globals.set, pm.collectionVariables.set, collectionVariables.set, pm.environment.set, environment.set, pm.variables.get, variables.get, pm.globals.get, globals.get, pm.collectionVariables.get, collectionVariables.get, pm.environment.get, environment.get, pm.iterationData.get, iterationData.get"
contextual_links:
  - type: section
    name: "Prerequisites"
  - type: link
    name: "Sending requests"
    url:  "/docs/sending-requests/requests/"
  - type: section
    name: "Additional Resources"
  - type: subtitle
    name: "Videos"
  - type: link
    name: "How to Use Variables in Postman"
    url: "https://youtu.be/BKLC-_C9fxE"
  - type: link
    name: "Intro to Postman | Chain Requests"
    url: "https://youtu.be/4fULCou_7Wc"
  - type: link
    name: "Manage CLI Environment Variables | Postman Level Up"
    url: "https://youtu.be/n8O2KP-Zx8I"
  - type: subtitle
    name: "Related Blog Posts"
  - type: link
    name: "Find and replace text, code, and variables"
    url: "https://blog.postman.com/find-and-replace-text-code-and-variables-in-postman/"
  - type: link
    name: "10 tips for working with variables"
    url: "https://blog.postman.com/10-tips-for-working-with-postman-variables/"
  - type: link
    name: "Securely Using API Keys in Postman"
    url: "https://blog.postman.com/how-to-use-api-keys/"
  - type: section
    name: "Next Steps"
  - type: link
    name: "Grouping requests in collections"
    url: "/docs/sending-requests/intro-to-collections/"
  - type: link
    name: "Intro to scripts"
    url: "/docs/writing-scripts/intro-to-scripts/"
  - type: link
    name: "Dynamic variables"
    url:  "/docs/writing-scripts/script-references/variables-list/"

warning: false

---

Variables enable you to store and reuse values in your requests and scripts. By storing a value in a variable, you can reference it throughout your collections, environments, and requests. If you need to update the value, you only have to change it in one place. Using variables increases your ability to work efficiently and minimizes the likelihood of error.

## Contents

* [Variables quick start](#variables-quick-start)
* [Understanding variables](#understanding-variables)
* [Variable scopes](#variable-scopes)
* [Variable types](#variable-types)
* [Defining variables](#defining-variables)
* [Accessing variables](#accessing-variables)
* [Sharing and persisting data](#sharing-and-persisting-data)
* [Logging variables](#logging-variables)
* [Using data variables](#using-data-variables)
* [Using dynamic variables](#using-dynamic-variables)
* [Next steps](#next-steps)

## Variables quick start

To try out a variable:

1. Select the __Environment quick look__ <img alt="External link icon" src="https://assets.postman.com/postman-docs/eye.jpg" width="24px" style="vertical-align:middle;margin-bottom:5px"> in the top right of Postman and select __Edit__ next to __Globals__.
1. Add a variable named `my_variable` and give it an initial value of `Hello`.
1. Select __Save__ and close the environment dialog.
1. Open a new request tab and enter `https://postman-echo.com/get?var={{my_variable}}` as the URL. Hover over the variable name and you'll see the value.
1. Select __Send__ and send the request. In the response, you'll see that Postman sent the variable value to the API.
1. Try changing the value in the Environment quick look and sending the request again.

Read on for more detail on how you can use variables in Postman.

## Understanding variables

A variable is a symbolic representation of data that enables you to access a value without having to enter it manually wherever you need it. This can be useful especially if you are using the same values in multiple places. Variables make your requests more flexible and readable, by abstracting the detail away.

For example, if you have the same URL in multiple requests, but the URL might change later, you can store the URL in a variable. If the URL changes, you only need to change the variable value and it will be reflected throughout your collection, wherever you've used the variable name. The same principle applies to any part of your request where data is repeated.

![Environment editor](https://assets.postman.com/postman-docs/environment-editor-new-v9.13.jpg)

<a href="https://assets.postman.com/postman-docs/reference-var-v9.jpg"><img alt="Reference Variable" src="https://assets.postman.com/postman-docs/reference-var-v9.jpg" width="300px"/></a>

Postman supports variables at different scopes, allowing you to tailor your processing to a variety of development, testing, and collaboration tasks. Scopes in Postman relate to the different contexts that your requests run in: within Postman, in collections, in environments, in Newman, and in the Collection Runner. You can use variables to pass data between requests and tests, for example if you are chaining requests using a collection.

> Postman will store environment and global variables as strings. If you’re storing objects or arrays, remember to `JSON.stringify()` them before storing, and `JSON.parse()` them when you retrieve them.

Variables in Postman are key-value pairs. Each variable name represents its key, so referencing the variable name allows you to access its value.

For example, if you have a base URL for requests stored in a variable named `base_url`, you can reference it in your requests using `{{base_url}}`. Whatever value is stored in the variable will be included wherever you've referenced the variable when your requests run. If the base URL value is `https://httpbin.org`, and is listed as part of the request URL using `{{base_url}}/get?customers=new`, Postman will send the request to `https://httpbin.org/get?customers=new`.

You can use environments to group sets of variables together and share them with collaborators, for example if you use one set of config details for your production server and another for testing. See [Managing environments](/docs/sending-requests/managing-environments/) for more on how you can incorporate environments into your team workflows.

## Variable scopes

Postman supports the following variable scopes:

* Global
* Collection
* Environment
* Data
* Local

<img alt="Variable Scope" src="https://assets.postman.com/postman-docs/var-scope.jpg" width="350px"/>

If a variable with the same name is declared in two different scopes, the value stored in the variable with narrowest scope will be used—for example if there is a global and a local variable both named `username`, the local value will be used when the request runs.

### Choosing variable scope

Variable scopes are suited to different tasks in Postman:

* __Global variables__ enable you to access data between collections, requests, test scripts, and environments. Global variables are available throughout a [workspace](/docs/collaborating-in-postman/using-workspaces/creating-workspaces/).

   > Since global variables can create confusion, you should only use them sparingly. For example, use a global variable to test something or when your project is at an early prototyping stage.
* __Collection variables__ are available throughout the requests in a collection and are independent of environments, and don't change based on the selected environment.
    > Collection variables are suitable if you are only using a single environment, for example for auth or URL details.
* __Environment variables__ enable you to tailor your processing to different environments, for example local development versus testing or production. Only one environment can be active at a time.
    > If you have only one environment, using collection variables can be more efficient. However environments enable you to specify [role-based access levels](/docs/sending-requests/managing-environments/#working-with-environments-as-a-team).
* __Local variables__ are temporary, and only accessible in your request scripts. Local variable values are scoped to a single request or collection run, and are no longer available when the run is complete.
    > Local variables are suitable if you need a value to override all other variable scopes but don't want the value to persist once execution has ended.
* __Data variables__ come from external CSV and JSON files to define data sets you can use when running collections with Newman or the Collection Runner.

![Variable Scopes](https://assets.postman.com/postman-docs/Variables-Chart.png)

## Variable types

Beyond scope, global and environment variables can be defined by type. The two variable types that you can configure for environment and global variables are:

* **Default type** is the default type that is automatically assigned to variables. This type is shown as plain text and has no additional properties.
* **Secret type** masks the [initial and current values](#specifying-variable-detail) for all workspace members and can be used to prevent unintentional disclosure of sensitive data, including API secrets, passwords, tokens, and keys.

Users with [editor](/docs/collaborating-in-postman/roles-and-permissions/) access on a workspace (for global variables) or environment (for environment variables) can opt to change these variables from default to secret type.

> Postman will store environment and global variables as default regardless of type.

To set the variable type, select the Environment quick look <img alt="Environment quick look icon" src="https://assets.postman.com/postman-docs/eye.jpg" width="24px" style="vertical-align:middle;margin-bottom:5px"> in the top right of Postman. Select **Edit** to the right of environment or global variables to open the editor.

<img alt="Environment editor" src="https://assets.postman.com/postman-docs/environment-editor-default-v9.13.jpg">

> You can also edit an environment by navigating to the workspace it resides in and selecting **Environments** from the left sidebar, then selecting your environment.

<!-- -->

> You must have [editor](/docs/collaborating-in-postman/roles-and-permissions/) access on a workspace (for global variables) or environment (for environment variables) to control variable type.

Select **default** next to the variable you'd like to change to open the dropdown menu, then select **secret** to update the variable type and save your changes to confirm.

<img alt="Environment editor" src="https://assets.postman.com/postman-docs/environment-editor-secret-v9.13.jpg">

All workspace members can view a secret variable's initial and current values by selecting the <img alt="Eye crossed out icon" src="https://assets.postman.com/postman-docs/eye-crossed-out.jpg" width="24px" style="vertical-align:middle;margin-bottom:5px"> eye symbol to the right of the variable.

Editors can change a variable's initial values, which are shared with collaborators, by selecting the <img alt="Eye crossed out icon" src="https://assets.postman.com/postman-docs/eye-crossed-out.jpg" width="24px" style="vertical-align:middle;margin-bottom:5px"> eye symbol to the right of the variable, then selecting the initial value. All collaborators can change a variable's current values by selecting the <img alt="Eye crossed out icon" src="https://assets.postman.com/postman-docs/eye-crossed-out.jpg" width="24px" style="vertical-align:middle;margin-bottom:5px"> eye symbol to the right of the variable, then selecting the current value.

Editors can change the variable type from secret to default at any time, and vice versa. When you change a variable's type from secret back to default, you must confirm by selecting **Change type**.

<img alt="Confirm unmark variable as secret" src="https://assets.postman.com/postman-docs/environment-change-type-confirmation-v9.13.jpg" width="400px">

## Defining variables

You can define variables in a variety of ways, depending on if you need [global, environment](#defining-global-and-environment-variables), or [collection](#defining-collection-variables) scope.

To create a variable at any scope from the request builder:

1. Select the data you need, for example in the address, parameters, headers, or body.

    <img src="https://assets.postman.com/postman-docs/set-as-var-prompt.jpg" alt="Set as variable" width="450px"/>

1. Choose **Set as variable** &gt; **Set as a new variable**.

    <img src="https://assets.postman.com/postman-docs/set-as-a-new-var.jpg" alt="Set as variable" width="300px"/>

1. Enter a **Name**, verify the **Value**, and select a scope from the drop-down list. Select **Set variable**.

    <img src="https://assets.postman.com/postman-docs/set-as-var-modal.jpg" alt="Set as variable" width="450px"/>

> Remember to delete variables you are no longer using.

### Defining global and environment variables

You can create and edit environment variables by selecting __Environments__ on the left of Postman, or using the __Environment quick look__ <img alt="External link icon" src="https://assets.postman.com/postman-docs/eye.jpg" width="24px" style="vertical-align:middle;margin-bottom:5px"> at the top right.

You can choose an environment in the drop-down list at the top right:

<img src="https://assets.postman.com/postman-docs/environment-selector-v9.13.jpg" alt="Environment selector" width="300px"/>

You can also activate an environment in the left sidebar, by selecting the check-mark button to make the environment _active_.

<img src="https://assets.postman.com/postman-docs/set-environment-active-left.jpg" alt="Environment Quick Look" width="350px"/>

The environment quick look shows the selected environment along with global variables in your workspace. You can edit the current value for an existing variable inline, by selecting the value. To add a variable, select __Edit__ next to the environment or global section.

You can only add and edit variables in environments if you have edit access to the environment as a whole. If you have view access, you can update the current value of existing variables only. Any variables you edit will only be accessible to you, and not available to collaborators in your [workspace](/docs/collaborating-in-postman/using-workspaces/creating-workspaces/).

See [Managing environments](/docs/sending-requests/managing-environments/) for more on working with environments in your team.

Once you have your scope selected, you can [specify the variable detail](#specifying-variable-detail).

You can also [define global and environment variables in scripts](#defining-variables-in-scripts).

### Defining collection variables

You can add collection variables when you create the collection or at any time after that. To create or edit a variable for an existing collection, select **Collections** in the left sidebar. Select a collection, and then select the **Variables** tab.

[![Edit Collection](https://assets.postman.com/postman-docs/collection-variables-v9.jpg)](https://assets.postman.com/postman-docs/collection-variables-v9.jpg)

> If you don't have edit access to a collection, you will see a __Request Access__ button. You won't be able to add new collection variables, update initial values, or persist values.
> You can edit the current value for local use, override the collection variable by using an environment variable with the same name, or [request access](/docs/collaborating-in-postman/requesting-access-to-collections/) to the collection for __Editor__ role.

You can also [define collection variables in scripts](#defining-variables-in-scripts).

### Specifying variable detail

You can add and edit variables at any time. All you need to include for a new variable is a name. You can choose to supply an initial value but can alternatively set it later, including from scripts. Use the checkbox to the left of a variable to activate or deactivate a variable.

Initial values are shared when you share a collection or environment. Current values are local and not synced or shared. See [Sharing and persisting data](#sharing-and-persisting-data) for more on local vs synced variables.

> You can download global variables as JSON from __Manage Environments__.

You can set response body values to variables by selecting text, right-clicking or Control-clicking, and choosing the relevant variable by name.

<img alt="Set Variable from Text" src="https://assets.postman.com/postman-docs/set-var-text.jpg" width="400px"/>

### Defining variables in scripts

You can set variables programmatically in your request scripts.

Use `pm.globals` to define a global variable:

```js
pm.globals.set("variable_key", "variable_value");
```

Use `pm.collectionVariables` to define a collection variable:

```js
pm.collectionVariables.set("variable_key", "variable_value");
```

Use `pm.environment` to define an environment variable (in the currently selected environment):

```js
pm.environment.set("variable_key", "variable_value");
```

> If you have view access but not edit access to an environment, your script code will only affect the current value, and won't be synced or shared with your team.

You can use `unset` to remove a variable:

```js
pm.environment.unset("variable_key");
```

Check out the [Sandbox Reference](/docs/writing-scripts/script-references/postman-sandbox-api-reference/) for more on scripting with variables.

## Defining local variables

Local variables are temporary values you set in your request scripts using the following syntax:

```js
pm.variables.set("variable_key", "variable_value");
```

Local variables don't persist between sessions, but allow you to override all other scopes temporarily, during the execution of a request or collection / monitor run. For example, if you need to process a temporary test value for a single request or collection run locally, and don't want the value to sync with your team or remain available when the request / collection has finished running, you can use a local variable.

## Accessing variables

You can use double curly braces to reference variables throughout the Postman user interface. For example, to reference a variable named "username" in your request auth settings, you could use the following syntax with double curly braces around the name:

```js
{{username}}
```

When you run a request, Postman will resolve the variable and replace it with its current value.

For example, you could have a request URL referencing a variable as follows:

```js
http://pricey-trilby.glitch.me/customer?id={{cust_id}}
```

Postman will send whatever value you currently have stored for the `cust_id` variable when the request runs. If `cust_id` is currently `3`, the request will be sent to the following URL including the query parameter:

```js
http://pricey-trilby.glitch.me/customer?id=3
```

Alternatively, you could have a request body that accesses a variable by wrapping its reference in double-quotes:

```js
{ "customer_id" : "{{cust_id}}" }
```

You can use variables in request URLs, parameters, headers, authorization, body, and header presets.

[![Variables in Request](https://assets.postman.com/postman-docs/var-auth-v8.jpg)](https://assets.postman.com/postman-docs/var-auth-v8.jpg)

When you hover over a variable you can see an overview of its current status. As you type variables into your requests, Postman will prompt you with any that are currently defined.

![Variable Prompt](https://assets.postman.com/postman-docs/var-prompt.jpg)

The prompt will indicate current value, scope (highlighted by color), and overridden status where relevant.

![Overridden Variable](https://assets.postman.com/postman-docs/overridden-var.jpg)

If a variable is unresolved, Postman will highlight it in red.

<img alt="Unresolved Variable" src="https://assets.postman.com/postman-docs/unresolved-var.jpg" width="250px"/>

### Using variables in scripts

You can retrieve the current value of a variable in your scripts using the object representing the scope level and the `.get` method:

```js
//access a variable at any scope including local
pm.variables.get("variable_key");
//access a global variable
pm.globals.get("variable_key");
//access a collection variable
pm.collectionVariables.get("variable_key");
//access an environment variable
pm.environment.get("variable_key");
```

> Using `pm.variables.get()` to access variables in your scripts gives you the option to change variable scope without affecting your script functionality. This method will return whatever variable currently has highest precedence (or narrowest scope).

## Sharing and persisting data

When you edit global, collection, and environment variables in Postman, you will see __Current Value__, __Persist__, and __Reset__ options for individual variables and for all variables (hover over a variable and select <img alt="Three dots icon" src="https://assets.postman.com/postman-docs/icon-three-dots-v9.jpg" width="18px" style="vertical-align:middle;margin-bottom:5px"> to persist individual values). These enable you to control what happens within your local instance of Postman, independently of how the data is synced with anyone sharing your workspace, requests, collections, and environments.

Your local session in Postman can use values that are transient and only visible to you. This lets you develop and test using private credentials or experimental values, without risk of exposing these details or affecting others on your team.

> For example, your team could have a shared API key and individual API keys. You could do experimental development work locally using your personal key, but use the shared key for team collaboration. Similarly, you could have a variable that represents exploratory work you're doing locally but aren't ready to share with the team. You can later choose to persist the local data so that others on your team can also access it.

When you create or edit a variable, you can enter both an initial and a current value. When you create a new variable in the UI, if you leave the current value empty, it will autofill with the initial value. If you specify a current value, it will be local only to your instance—the __Persist__ option lets you push your current value to the shared data, updating the initial value to match the current value.

> You can only edit the initial value of an environment variable if you have edit access to the environment itself. Without edit access to the environment, you can only edit the current value and your edit won't be visible to anyone sharing your [workspace](/docs/collaborating-in-postman/using-workspaces/creating-workspaces/).

Using __Persist__ will make your current value sync with Postman's servers and be reflected for anyone sharing your collection or environment. To reset your current local values to reflect the initial (shared) values, use __Reset__.

You can edit a current value inline from the environment quick look.

See [Managing environments](/docs/sending-requests/managing-environments/#creating-environments) for more on working with variables as a team.

> Local and data variables only have current values, which don't persist beyond request or collection runs.

## Logging variables

You can log variable values to the [Postman Console](/docs/sending-requests/troubleshooting-api-requests/) while your requests run. Open the console from the button on the bottom left of Postman, or from the __View__ menu. To log the value of a variable, use the following syntax in your script:

```js
console.log(pm.variables.get("variable_key"));
```

[![Logging Variable](https://assets.postman.com/postman-docs/log-var-v8.jpg)](https://assets.postman.com/postman-docs/log-var-v8.jpg)

## Using data variables

The Collection Runner lets you import a CSV or a JSON file, and use the values from the data file inside requests and scripts. You can't set a data variable inside Postman because it's pulled from the data file, but you can access data variables inside scripts, for example using `pm.iterationData.get("variable_name")`.

See [working with data files](/docs/running-collections/working-with-data-files/) and the [Sandbox API reference](/docs/writing-scripts/script-references/postman-sandbox-api-reference/) for more.

## Using dynamic variables

Postman provides dynamic variables that you can use in your requests.

Examples of dynamic variables are as follows:

* `{{$guid}}` : A v4 style guid
* `{{$timestamp}}`: The current timestamp (Unix timestamp in seconds)
* `{{$randomInt}}`: A random integer between 0 and 1000

See the [Dynamic Variables](/docs/writing-scripts/script-references/variables-list/) section for a full list.

> To use dynamic variables in pre-request or test scripts, use `pm.variables.replaceIn()`, for example `pm.variables.replaceIn('{{$randomFirstName}}')`.

## Next steps

For an overview of how you can manage and share variable data sets, see [Managing environments](/docs/sending-requests/managing-environments/). Check out [Intro to scripts](/docs/writing-scripts/intro-to-scripts/) for more on using variables in your request scripting, and [Grouping requests in collections](/docs/sending-requests/intro-to-collections/) for more on how you can use data between requests.

---
title: Auth0 Aurelia SDK Tutorial
description: This tutorial will show you how to use the Auth0 Aurelia SDK to add authentication and authorization to your web app.
name: Aurelia
alias:
  - aurelia
  - aureliajs
language:
  - Javascript
framework:
  - Aurelia
image:
tags:
  - quickstart
snippets:
  configurehttp: client-platforms/aurelia/configurehttp
  constructorexpiry: client-platforms/aurelia/constructorexpiry
  dependencies: client-platforms/aurelia/dependencies
  http: client-platforms/aurelia/http
  jwtdecode: client-platforms/aurelia/jwtdecode
  login: client-platforms/aurelia/login
  logout: client-platforms/aurelia/logout
  routing: client-platforms/aurelia/routing
  setup: client-platforms/aurelia/setup
  template: client-platforms/aurelia/template
  tokenisexpired: client-platforms/aurelia/tokenisexpired
alias:
  - aurelia
seo_alias: aurelia
---

## Aurelia SDK Tutorial

::: panel-info System Requirements
This tutorial and seed project have been tested with the following:
* NodeJS 5.0.0
* JSPM 0.16.27
* Aurelia-framework 1.0.0-beta.1.1.0
:::

<%= include('../_includes/_package', {
  pkgRepo: 'auth0-aurelia',
  pkgBranch: 'master',
  pkgPath: null,
  pkgFilePath: null,
  pkgType: 'js'
}) %>

**If you have an existing application, follow the steps below.**

${include('./\_callback')}

### 1. Add the Auth0 Scripts

Add **Lock** to your `index.html` file and set the viewport.

${snippet(meta.snippets.dependencies)}

### 2. Import Dependencies and Set Up Auth0Lock

Later we'll see how to make authenticated HTTP requests, and for that we'll need `HttpClient` from `aurelia-fetch-client`. We also need to create a new instance of `Auth0Lock`.

${snippet(meta.snippets.setup)}

We also set the `isAuthenticated` property to false to start with, but this value will be changed later on to reflect the user's authentication status.

### 2. Set Up the Login and Logout Methods

The `login` and `logout` methods will be bound to button clicks in the template.

We first need to set up the buttons in our template. At the same time, we'll create a button that will be used for authenticated HTTP requests.

${snippet(meta.snippets.template)}

The `login` method will show the Lock widget and save the user's profile and JWT in local storage if authentication is successful and set `isAuthenticated` to true.

${snippet(meta.snippets.login)}

To log the user out, we just need to remove the their JWT and profile from local storage, and then set `isAuthenticated` to false.

${snippet(meta.snippets.logout)}

__Note:__ There are multiple ways of implementing login. The example above displays the Lock Widget. However you may implement your own login UI by changing the line `<script src="${widget_url_no_scheme}"></script>` to `<script src="//cdn.auth0.com/w2/auth0-6.7.js"></script>`.

### 3. Make Secure Calls to an API

To make secure calls to an API, attach the user's JWT as an `Authorization` header to the HTTP request. This is done in the `RequestInit` object as the second argument to the `fetch` call.

${snippet(meta.snippets.http)}

### 4. Configure All HTTP Calls to be Secure

If you wish to attach the user's JWT as an `Authorization` header on all HTTP calls, you can configure the `HttpClient` to do so.

${snippet(meta.snippets.configurehttp)}

You can then remove the `RequestInit` object (the second argument of `fetch`) from individual HTTP calls.

### 5. Optional: Decode the User's JWT to Check Expiry

Checking whether the user's JWT has expired is useful for conditionally showing or hiding elements and limiting access to certain routes. This can be done with the `jwt-decode` package and a simple function. First, install the package.

${snippet(meta.snippets.jwtdecode)}

Next, we can create a utilities file that will have a function that decodes the JWT and checks whether it has expired.

${snippet(meta.snippets.tokenisexpired)}

With this method in place, we can now call it in the constructor so that the user's authentication state is preserved if the page is refreshed.

${snippet(meta.snippets.constructorexpiry)}

### 6. Check Whether a Route Can Be Activated

Aurelia's `canActivate` method can be used to check whether a route can be navigated to. If the user's JWT has expired, we don't want them to be able to navigate to private routes.

${snippet(meta.snippets.routing)}

This hook will redirect the user to some other route (`public` in this case) if the user's JWT has expired.

### 7. All done!

You have completed the implementation of Login and Signup with Auth0 and Aurelia!

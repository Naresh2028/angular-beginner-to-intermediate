# Why Modules Exist

Let’s start with a simple scenario.

Imagine this small Angular app

````sql
HomePage
LoginForm
HeaderBar
Footer
````

As long as these are a few pieces, you don’t worry where they are.

But now imagine your app grows:

````sql
User Profile
Shopping Cart
Orders History
Admin Dashboard
Settings
Notifications
Product Catalog
Payments
Checkout
Analytics
...
````

Now you start organizing folders.

Some developers put things here, some put things there.

Without a system, chaos appears:

- Where should I put this component?

- Who owns this shared feature?

- Which components belong together?

- What can reuse what?

Angular could have said:

- “Just use folders!”

But folders are only a human thing, not a language feature.

Angular needed a way to understand what belongs together — not by names, not by folders, but by declaration.

So they invented "NgModule".


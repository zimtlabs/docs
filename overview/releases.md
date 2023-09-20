---
layout:
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# 🔢 Releases

***

## Environments

For every project we will have 3 main environments with automatic deployment.

* **Production** (master _branch_)
* **Staging** (staging _branch_)
* **Development** (dev _branch_)

**Production** environment is used for live version of the application, it is always a latset stable relese

**Staging** environment is used for testing of the newest version. After testing and bug fixes if staging environment is stable when time comes we can merge it to the production.

**Development** environment is used by devs to develop the aplication.

Internaly we are using discord channel for notifications for deploys.

***

### Additional environments (optional)

In some situations we might have additional environments that will meet our needs or some external services/ frameworks.

For example we might have an adidtional environment for `storybook` or if we are using graphql `graphiql explorer`...

***

## **Versioning**

We should version a project in a format of `X.Y.Z-label` :

* **X (Major version)** - Incremented whenever major changes are made like architectual change.
* **Y (Minor version) -** Incremented whenever minor changes are made which does not break the API like a new feature
* **Z (Patch version)** - Incremented with bug fixes
* **Label** is an **optional** pre-relese label that represents current stage of the project like: beta, alpha,…

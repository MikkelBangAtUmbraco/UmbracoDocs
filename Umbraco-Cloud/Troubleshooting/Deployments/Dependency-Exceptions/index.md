---
versionFrom: 7.0.0
versionTo: 10.0.0
---

# Dependency Exception

if you delete a content node which might have a reference to another node and you delete the node you are referencing to, you will encounter the following error when trying to transfer the node:

![Dependecy exception](images/dependency-exception-updated.png)

The error indicates that on the `Home page`, a picker, for example, refers to another content item. This other content item has either been deleted or is in the recycle bin in the environment you're deploying from.

:::tip
If you try and delete a node that references another one, you will be warned in the backoffice that it has a reference and you might encounter issues if you delete it:
![dependency warning](images/dependency-exception-warning.png)
:::

## How to fix your dependency error

To resolve the issue, find the Contact page (hint: the nodeId can be used in the search field in the top-left of the backoffice) and publish it again. This should remove the reference to the node that is no longer available. Transferring the Contact page node should now succeed.

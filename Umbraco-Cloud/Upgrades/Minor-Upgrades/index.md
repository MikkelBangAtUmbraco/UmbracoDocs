---
versionFrom: 7.0.0
versionTo: 10.0.0
---

# Minor version upgrades

When minor version upgrades are available, your Umbraco site will not be auto-upgraded to this version. You will need to press the Upgrade button in the Umbraco Cloud portal to perform the upgrade. This will upgrade your Development environment so you can test how everything works on a Cloud environment before pushing the upgrade to your Live site.

This workflow applies to all products on Umbraco Cloud: Umbraco CMS, Umbraco Forms, and Umbraco Deploy.


For Starter plans, you will need to add a Development environment first before you can perform the semi-automatic upgrade. Find pricing details for Umbraco Cloud Starter plans on our [website](https://umbraco.com/pricing/).

<iframe width="800" height="450" title="How to upgrade an Umbraco Cloud environment to a new minor version" src="https://www.youtube.com/embed/mMk2VM_5N-I?rel=0" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen></iframe>

## Troubleshooting Automated minor upgrades

Umbraco Cloud supports doing minor upgrades to your projects in an automated manner. The feature is available when a new minor version of Umbraco is released (i.e. 9.5.0 or 9.6.0).

The upgrade will cover most issues it encounters, but at certain Umbraco configurations, it needs some manual intervention. This is usually related to custom code being dependent on some APIs that have either changed or been removed for the new minor upgrade.

In general, if anything should fail during this process, you can reach out for support, using the in-app chat in the bottom right corner. We will help you through the upgrade process, should anything happen.


## Database upgrade failing

Symptoms, feedback given from the upgrade process: **Unable to run the Umbraco installer**

The first step in the process, after having updated all the files, is to call the Umbraco install engine in order to get its  database updated to support the new version. As this step is the first time the site gets requested after the updated files are run, it may fail. The reason is often code that is incompatible with the upgraded files.

It can be code that references APIs that have been deprecated, or code that has some strong references to specific versions.
If the error is clear, then it will be shown on the screen, as it will be a typical ASP.NET error message (YSOD). You should request the site, and check the error it shows.
If the error isn't descriptive, then it is time to clone the repository to your local machine, and fix the issue. The usual suspects would be:

- The code you have running is referencing an API that has been changed, that is being modified, is obsolete, or removed.
- The `web.config` had assembly bindings for a specific DLL version that doesn't exist anymore. During the upgrade process, we do update the references we are shipping, but there might be something missing.

Once you have the site running locally, you should push your changes to the repository. This will update the site, and it should now be in a running state.

The upgrade process left off when it was needing three more steps. These three steps now need to be done manually.

1. Complete the installer

    To complete the installer, you should visit the site on its url. it will be like `https://dev-mysite.s1.umbraco.io. This will show you the installer screen, where you should insert your backoffice credentials, and follow the process. It will run through a few steps, and afterward, Umbraco will be updated to the latest version.
2. Export the metadata files.

    The second thing you need to do is to regenerate the metadata files used for transferring items like document types, data types, and media types. This is done by accessing the Power tools(Kudu) on the project, opening the cmd prompt, and browse to the wwwroot/data folder.
    Once there, you need to enter `echo > deploy-export`. This will generate the needed files for the upgraded site to work with Umbraco Deploy.
3. The last thing to do is to go to the `/site/locks` folder (still through Kudu) and rename the file called `upgrading` to `upgraded-minor` - rename the file by typing `ren upgrading upgraded-minor`. This will indicate to Umbraco Cloud, that the development environment is now ready to deploy all its changes to the next environment.

Before deploying the upgrade to the next environment, you should verify that everything looks as expected on the development environment.

---
title: Azure DevOps/TFS Issue Tracking Integration
description: Configure Azure DevOps/TFS issue tracking with Octopus.
---

**Octopus** supports integration with **Azure DevOps/TFS** to allow Octopus to display links to any work items attached to Azure DevOps/TFS builds. The integration adds Azure DevOps/TFS work item titles and links to your releases and deployments, and it can automatically retrieve release notes from Azure DevOps/TFS work item comments to help automate the release note process. This feature builds upon the functionality to [track metadata and work item](/docs/api-and-integration/metadata/index.md) information through your CI/CD pipeline.

![Octopus release with Azure DevOps/TFS work items](octo-jira-release-details.png "width=500")

![Octopus deployment with generated release notes](octo-jira-release-notes.png "width=500")

This page describes how to configure this functionality for Octopus and Azure DevOps/TFS.

## Connecting Azure DevOps/TFS and Octopus Deploy

The Azure DevOps Issue Tracker extension is very easy to configure with a small number of settings.

1. Configure the Azure DevOps Issue Tracker extension.

    In the Octopus web portal, navigate to **{{Configuration,Settings,Azure DevOps Issue Tracker}}** and set the **Personal Access Token** (if required) and the **Azure DevOps Base URL** where the token applies.

    Ensure the **Is Enabled** property is set as well.

2. Configure the Release Note Options (optional).

    - **Release Note Prefix**: If specified, Octopus will check a build's associated work items for comments that start with the given prefix text, and create a release note from whatever appears after the prefix. This is provided to the [release notes template](/docs/api-and-integration/metadata/release-notes-templates.md) as the work item link's description. If no comment is found with the prefix then Octopus will default back to using the title for that work item.

    For example, a prefix of `Release note:` can be used to indicate a customer-friendly work item title, rather than a technical feature or bug fix title.

When configured, this integration will retrieve work items attached to Azure DevOps/TFS builds, add the details to your releases and deployments, and generate release notes automatically.

## Troubleshooting

### Error message: "Unable to obtain some work item data."

The rest of the message should give an indication why, but this typically indicates a failure to do one of the following:

* contact Azure DevOps/TFS over the network
* authenticate using the specified Personal Access Token to read scopes 'Build' and 'Work Items'
* retrieve build and work item data and work item comments
* interpret the data

If you can't resolve the problem and want to create a release, some options are:

* disable the Azure DevOps Issue Tracker extension under **{{Configuration,Settings,Azure DevOps Issue Tracker}}**
* push packages without metadata, for example by disabling the build task **Push Package Metadata to Octopus**

## Next

 - Learn about other [Metadata and Work Items](/docs/api-and-integration/metadata/index.md).

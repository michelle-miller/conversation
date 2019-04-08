---

copyright:
  years: 2015, 2019
lastupdated: "2019-04-08"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:deprecated: .deprecated}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# Upgrading

Learn how to upgrade your service plan.
{: shortdesc}

This version of the {{site.data.keyword.conversationshort}} documentation is deprecated. Go [here](https://cloud.ibm.com/docs/services/assistant?topic=assistant-upgrade) instead.
{: deprecated}

## Upgrading your plan
{: #plan-upgrade}

You can explore the {{site.data.keyword.conversationshort}} [service plan options ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/catalog/services/watson-assistant-formerly-conversation){: new_window} to decide which plan is best for you.

To upgrade your plan, complete these steps:

1.  From the {{site.data.keyword.Bluemix_notm}} menu, select **Services** > **Dashboard**.
1.  Select the service instance that you want to upgrade to open it.
1.  Click **Plan** from the navigation pane.
   From here, you can see your current plan and other available plan options, and make changes.

For answers to common questions about subscriptions, see the [Managing billing and usage ![External link icon](../../icons/launch-glyph.svg "External link icon")](/docs/billing-usage/how_charged.html){: new_window}.

For more information about {{site.data.keyword.Bluemix_notm}} services, see [{{site.data.keyword.Bluemix_notm}} services information](services-information.html).

## Upgrading your workspace
{: #upgrade-workspace}

The {{site.data.keyword.conversationshort}} service regularly adds and updates features. While some of these changes are automatically applied to your workspace, updates that have a large impact do require a manual update.

An upgrade is only available for your workspace if the upgrade icon (![upgrade icon](images/upgrade.png)) is displayed.

**Note**: After you upgrade a workspace, you cannot revert your workspace to a previous version.

To upgrade your workspace, complete the following steps:
1.  [Duplicate your workspace](configure-workspace.html#exporting-and-copying-workspaces).
2.  Upgrade the duplicate workspace.

    When you upgrade your workspace, the latest version of the API is enabled in the tool, and the "Try it out" pane begins to use the newest features.
3.  Test the upgraded workspace.
4.  After evaluating the duplicate workspace to understand how the upgrade will impact your application, apply the upgrade to your primary workspace.
5.  Upgrade your application. To do so, change the message API calls it uses to specify the latest API version. For API version details, see the [release notes](release-notes.html#service-api-versioning).

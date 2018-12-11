---

copyright:
  years: 2015, 2018
lastupdated: "2018-11-08"

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

# IBM Cloud services information
{: #services-information}

The assistant is a hosted service that is managed by IBM Cloud, which means you do not need to worry about maintaining the infrastructure to support the service; it is taken care of for you.
{: shortdesc}

This version of the {{site.data.keyword.conversationshort}} documentation is deprecated. Go [here](https://console.bluemix.net/docs/services/assistant/services-information.html) instead.
{: deprecated}

## Service details
{: #service-details}

Explore the {{site.data.keyword.conversationshort}} [service plan options ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/catalog/services/watson-assistant-formerly-conversation){: new_window}.

### Limits by artifact
{: #limits}

For information about artifact limits per plan, see these topics:

- [Workspaces](configure-workspace.html#workspace-limits)
- [Dialog nodes](dialog-build.html#dialog-node-limits)
- [Intents](intents.html#intent-limits)
- [Entities](entities.html#entity-limits)
- [Logs](logs_convo.html#log-limits)

### API call limits
{: #api-limits}

The number of API calls allowed per instance depends on your service plan. See your plan description for details.

If you have a Lite plan and reach your API call limit, but the logs show that you have made fewer calls than expected, remember that the Lite plan stores log information for only 7 days.

If you want to upgrade from one plan to another, see [Upgrading](upgrading.html).

## Data centers
{: #regions}

IBM Cloud has a network of global data centers that provide performance benefits to its cloud services. See [IBM Cloud global data centers ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/cloud/data-centers/){: new_window} for more details.

{{site.data.keyword.cloud_notm}} changed from managing user access with Cloud Foundry to using token-based Identity and Access Management (IAM) authentication. IAM was rolled out in different locations at different times.

You can create {{site.data.keyword.conversationshort}} service instances that are hosted in the following data center locations:

| Location    | Location code | Authentication type | IAM adoption date | Notes |
|-------------|---------------|---------------------|-------------------|-------|
| Dallas      | us-south      | IAM                 | 30 October 2018 | N/A |
| Frankfurt   | eu-de         | IAM                 | 30 October 2018 | N/A |
| Sydney      | au-syd        | IAM                 | 7 May 2018 | Instances created before May 7 were syndicated to Dallas |
| Tokyo       | jp-tok        | IAM                 | 8 November 2018 | N/A |
| United Kingdom |  eu-gb     | N/A                 | N/A        | Instances are syndicated to Dallas |
| Washington DC  | us-east    | IAM                 | 14 June 2018 | N/A |
{: caption="Data center locations" caption-side="top"}

## Authenticating API calls
{: #authenticate-api-calls}

The authentication mechanism used by your service instance impacts how you must provide credentials when making an API call.

1.  Get the service credentials.

    - Click the service instance on the [Dashboard ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/dashboard/apps?category=ai){: new_window}.

    - Click to open your service instance, click **Service credentials**, and then click **View credentials**.

      **Cloud Foundry credentials**

      ![Shows the service credentials page for Cloud Foundry-hosted instances.](images/cf-cred-ui.png)

      **IAM credentials**

      ![Shows the service credentials page for IAM-hosted instances.](images/iam-creds.png)

1.  Use these credentials in your API call.

    **Cloud Foundry API call**

    Provide your username and password credentials.

    ```curl
    curl -X GET \
    --user {username}:{password} \
    'https://gateway.watson.net/assistant/api/v1/workspaces?version=2018-09-20'
    ```
    {: codeblock}

     **IAM API call**

    - The base url must include the location. Use the syntax `gateway-<location>.watsonplatform.net` to specify the location in which you created the service instance. The location codes are listed in the *Data center locations* table.
    - Provide the appropriate type of token in the header. You can pass either a bearer token or an API key.

      - Tokens support authenticated requests without embedding service credentials in every call. The following example shows a bearer token being used.

        ```curl
        curl -X GET \
        'https://gateway-syd.watsonplatform.net/assistant/api/v1/workspaces?version=2018-09-20' \
        --header 'Authorization: Bearer eyJhbGciOiJIUz......sgrKIi8hdFs'
        ```
        {: codeblock}

      - API keys use basic authentication. The following example shows an apikey being used.

        ```curl
        curl -X GET -u "apikey:3Df... ...Y7Pc9" \
        'https://gateway-us-east.watsonplatform.net/assistant/api/v1/workspaces?version=2018-09-20' \
        ```
        {: codeblock}

        **Note**: When you use any of the Watson SDKs, you can pass the API key and let the SDK manage the lifecycle of the tokens.

        IAM resources cannot be managed with the Cloud Foundry Command Line Interface (CLI). For example, Cloud Foundry CLI commands (beginning with `cf`) that create or manage service instances do not work with instances hosted in locations using IAM. Instead, you must use the IBM Cloud CLI and its associated commands. See [Commands for managing resource groups and resources](https://console.bluemix.net/docs/cli/reference/bluemix_cli/bx_cli.html#commands-for-managing-resource-groups-and-resources) for more details.

        See [Authenticating with IAM tokens ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/docs/services/watson/getting-started-iam.html){: new_window} for more information.

    For examples, see  [Authentication ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/watson/developercloud/assistant/api/v1/node.html?node#authentication){: new_window} for your language in the API reference.

For information about the data centers in which other IBM Cloud services are hosted, see [Services by region ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/docs/resources/services_region.html#services_region){: new_window}.

## Terms and security
{: #terms}

To learn more about service terms and data security, read the following information:

- [Service terms ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/software/sla/sladb.nsf/sla/home?OpenDocument){: new_window}
- [Data security and privacy ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/software/sla/sladb.nsf/sla/csdsp?OpenDocument){: new_window}
- [Information security](information-security.html)

See [Platform overview ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://console.bluemix.net/docs/overview/ibm-cloud.html#overview){: new_window} for more information about IBM Cloud.

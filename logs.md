---

copyright:
  years: 2015, 2018
lastupdated: "2018-05-30"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# About the Improve component

The Improve component of {{site.data.keyword.conversationshort}} provides a history of interactions with users of your application. You can use this history to improve your application's understanding of users' inputs.
{: shortdesc}

Sections of the **Improve** panel:

* [Overview](logs_oview.html): A summary of conversations that users had with your application.
* [User conversations](logs_convo.html): A list of individual user utterances. You can update intents and entities while viewing an individual user utterance. **Note**: A single user conversation may consist of multiple utterances.
* [Recommendations](logs_recommend.html): Ways to improve your system. Available only to Premium users.

## Improving across applications
{: #deploy_id}

To understand how to use utterance data to make improvements across applications, it is helpful to review the following definitions associated with the {{site.data.keyword.conversationshort}} service:

* ***Instance***: Your deployment of {{site.data.keyword.conversationshort}}, accessible with unique credentials. A {{site.data.keyword.conversationshort}} instance may be made up of multiple applications.
* ***Application/Bot***: An application - often referred to as a 'bot' - is one model of your {{site.data.keyword.conversationshort}} content.
* ***User***: A user is anyone who interacts with your application; often these are your customers.
* ***Utterance***: An utterance is a single message a user sends to the application.
* ***Conversation***: A set of utterances consisting of the messages that an individual user sends to your application, and the messages your application responds with.
* ***Conversation ID***: A unique identifier used to link related exchanges. This identifier is sent using the `/message` API, by including the Conversation ID inside the metadata object in your [context ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#message).
* ***Workspace ID***: The unique identifier of an application
* ***Deployment ID***: Unique labels that are passed with user utterances to help identify the deployment environment that the utterances come from.
<!-- * ***Customer ID***: A unique label that can identify a specific user of your application. -->

Creating an application is a very iterative process. While you develop your application, you use the *Try it out* pane to verify that your application recognizes the correct intents and entities in test inputs, and to make corrections as needed.

In the **Improve** panel, you can view information about actual application interactions with your users, and make similar corrections to improve the accuracy with which intents and entities are recognized by your application. It is difficult to know exactly *how* your users will ask questions, or what random utterances they may make, so it is important to frequently visit the **Improve** panel, in order to improve your applications.

For a {{site.data.keyword.conversationshort}} instance that includes multiple applications, there may be times when it is useful to use utterance data from one application to improve another application within that same instance. **Note**: If you are a {{site.data.keyword.conversationshort}} Premium user, your premium instances can optionally be configured to allow access to log data from applications across your different premium instances.

As an example, say you have a {{site.data.keyword.conversationshort}} instance named *HelpDesk*. You may have both a Production application and a Development application in your HelpDesk instance. When working in the Development application, you can filter utterances based on the `Deployment ID` for the Production application, so that you are using Production application utterances to improve your Development application.

![Data source link](images/data_source_1.png)

Any edits you then make within the Development application will only affect the Development application, even if you’re using Production application utterances. See [Selecting a data source](logs_convo.html#selecting-a-data-source) for additional information.

To specify the deployment ID for an utterance sent using the `/message` API, include the deployment property inside the metadata object in your [context ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#message){: new_window}, as in this example:

```json
"context" : {
  "metadata" : {
       "deployment": "HelpDesk-Production"
  }
}
```
{: codeblock}

## Enabling user metrics
{: #user_id}

User metrics allow you to see, for example, the number of unique users who have engaged with your application/bot, or the average number of conversations per user over a given time interval on the [Overview page](logs_oview.html). User metrics are enabled by using a unique `User ID` parameter.

To specify the `User ID` for an utterance sent using the `/message` API, include the `user_id` property inside the metadata object in your [context ![External link icon](../../icons/launch-glyph.svg "External link icon")](https://www.ibm.com/watson/developercloud/assistant/api/v1/curl.html?curl#message){: new_window}, as in this example:

```json
"context" : {
  "metadata" : {
       "user_id": "{UserID}"
  }
}
```
{: codeblock}

<!-- ### Querying data
Use the `/logs` API `filter` parameter to search an application log for specific user data. For example, to search for data specific to a `User ID` that matches `my_best_customer`, the query might be:

```
curl -X GET
 --user {username}:{password}
 --data 'https://gateway.watson.net/conversation/api/v1/workspaces/{workspaceID}/logs?version=2018-02-16&filter=(language::en,request.header.metadata.user_id::my_best_customer)'
```
{: codeblock}

See the [Filter query reference](filter-reference.html) for additional details. -->

<!-- ## Deleting utterance data from specific users
{: #customer_id}

There might come a time when you want to completely remove a set of utterances between a specific user and your {{site.data.keyword.conversationshort}} instance. For example, under the European Union General Data Protection Regulation (GDPR), individuals are entitled to have their personal information, which may be included in utterances, removed upon request. Those utterances will then no longer be available when you [select a data source](logs_convo.html#select-source) to improve understanding.

### Prerequisite
To delete utterances which may contain personal information for one or more individuals, you first need to associate an utterance with a unique `Customer ID` for each individual that may have personal information in the utterance. To specify the `Customer ID` for any utterance sent using the `/message` API, include the `X-Watson-Metadata: customer_id` property in your header. You can pass multiple `Customer ID` entries with semicolon separated `field=value` pairs, using `customer_id`, as in the following example:

```
curl -X POST
 --user {username}:{password}
 --header
   'Content-Type: application/json'
   'Accept: application/json'
   'X-Watson-Metadata: customer_id={first-customer-ID};customer_id={second-customer-ID}'
 --data '{"input":{"text":"hello"}}' 'https://gateway.watson.net/conversation/api/v1/workspaces/{workspaceID}/message?version=2017-05-26'
```
{: codeblock}

**Note**: The `customer_id` string cannot include the semicolon (`;`) or equal sign (`=`) characters.

**Note**: You are responsible for managing `Customer ID` parameters, and ensuring that each parameter is unique across your customers.

### Deleting data
To delete utterances with personal information for one or more individuals, you provide the `customer_id` semicolon separated `field=value` pairs to the `user_data` parameter.

**IMPORTANT**: Specifying a `customer_id` will delete *all* utterances with that `customer_id` across your entire {{site.data.keyword.conversationshort}} instance, not just within one application.

As an example, to delete user ABC's data and user XYZ's data from your {{site.data.keyword.conversationshort}} instance, send the following cURL command:

```
curl -X DELETE
 --user {username}:{password}
 --data 'https://gateway.watson.net/conversation/api/v1/user_data/customer_id=abc;customer_id=xyz?version=2017-05-26'
```
{: codeblock}

Each example returns an empty JSON object `{}`. -->
